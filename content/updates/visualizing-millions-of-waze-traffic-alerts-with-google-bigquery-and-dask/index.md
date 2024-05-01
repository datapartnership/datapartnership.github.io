+++
date = 2022-02-08T00:00:00.000+00:00
title = "Visualizing Millions of Waze Traffic Alerts with BigQuery, H3 and Dask"
authors = ["Gabriel Vicente"]
categories = ["Tutorial"]
dev_partner = ["World Bank"]
partner= ["Waze"]
tags = ["Transport", "Urban Development"]
links = [
  "https://www.waze.com/wazeforcities",
  "https://docs.datapartnership.org/partners/waze/README.html",
  "https://docs.google.com/document/d/1oBEtDp7OULpVVa_MOS-060S7pO_TZPlFO-4XmSKgfV8/view",
]
+++

[Waze for Cities](https://www.waze.com/wazeforcities), a **Data Partner** of the [Development Data Partnership](https://datapartnership.org), connects government partners to high quality, frequent and granular traffic data extremely valuable in informing public sector, transport and urban planning as well as addressing the United Nations's [Sustainable Development Goals](https://sdgs.un.org/goals). Even so, sucessfully leveraging these alternative and often large data sources may pose challenges in building capacity adopting best available data, tools and practices.

In this first installlment of a *Partnership* series, we demonstrate how to analyze and visualize Waze for Cities data using [Google BigQuery](https://cloud.google.com/bigquery), [H3](https://h3geo.org) and [Dask](https://dask.org).

## Waze for Cities

[Waze](https://waze.com) has become an indispensable platform where over 140 million monthly drivers check and report accidents, slow downs, incidents and irregularities, adding up to billions of precious data points that could be better repurposed for improving public sector, transport and urban planning.

![](waze-for-cities.png)

Since 2014, Waze has shared these data freely for government partners as part of the [Waze for Cities](https://www.waze.com/wazeforcities) program and providing a myriad of data and tools via [Google Cloud](https://support.google.com/waze/partners/answer/10715739). The available data feeds are **Traffic Alerts**, **Traffic Accidents** and **Traffic Irregularities**.

<figure align="center">
  <img src="waze-cloud-resources.png"/>
  <figcaption> Cloud Resources for Waze for Cities Partners</figcaption>
  <figcaption> Source: https://support.google.com/waze/partners/answer/10715739</figcaption>
</figure>

Next, in this example, we will explore and visualize millions of Waze Traffic Alerts.

## Visualizing Waze Traffic Alerts

**Waze Traffic Alerts** include all traffic events and incidents reported by Waze users through the mobile application over a selected period of time, categorized into types, e.g. *ACCIDENT* or *JAM*, and subtypes, e.g. *JAM_MODERATE_TRAFFIC*, *JAM_HEAVY_TRAFFIC*, *JAM_STAND_STILL_TRAFFIC* or *JAM_LIGHT_TRAFFIC*. See [documentation](https://docs.datapartnership.org/partners/waze/examples/waze.html#alerts-types).

Waze makes its traffic data available through a historical archive on Google Cloud or a *live* GeoRSS feed that is updated every 2 minutes. For example, see below a live feed snapshot for Kiev, Ukraine.

<figure align="center">
  <img src="waze-alerts.png"/>
  <figcaption> Waze Alerts reported in Kiev, Ukraine at 21:00 UTC on February 7th, 2022. Data provided by Waze App. Learn more at waze.com</figcaption>
</figure>

Check out the live sneak peek on [visualizations](https://docs.datapartnership.org/pages/visualizations.html).

## Exporting Waze Data from Google BigQuery

Starting in 2019, Waze has been providing access to a historical archive of traffic data on Google Cloud's **BigQuery**. [BigQuery](https://support.google.com/waze/partners/answer/10715739) is a fully-managed data tool that enables interactive SQL-like queries, while heavy lifting behind the curtains, and facilitating layering of Waze data with other datasets.

Using BigQuery's [geography functions](https://cloud.google.com/bigquery/docs/reference/standard-sql/geography_functions), we select a time frame and Region of Interest (RoI) to be exported onto compressed and partitioned files.

<figure align="center">
  <img src="waze-big-query.png"/>
  <figcaption> Waze Alerts available via Google BigQuery, filtering São Paulo, Brazil.</figcaption>
</figure>

## Processing Waze Data with Dask

After exporting the data files (compressed CSVs), we have to prepare to visualize the large number of traffic alerts, in the order of millions. For that purpose, we leverage all power of **Dask**. [Dask](https://dask.org) is a Python library build on top of [NumPy](https://numpy.org/) and [pandas](https://pandas.pydata.org/). In addition to the familiar interface - for instance, reading multiple data files, decompressing and much more with a succint syntax and fast implementation -  the library enables embarrassing parallelism in a very convenient and efficient way.

Let's import the files into [Dask DataFrame](https://docs.dask.org/en/stable/dataframe.html),

```python
ddf = dd.read_csv(
        "s3://wbg-waze/bq/BR/alerts/2021/*.csv.gz",
        blocksize=None,
        compression="gzip",
)
```

Importantly, we make sure to cast proper data types, parsing the datetime into `datetime64[ns]`. Also, by using the `dt` accessor, we calculate additinal features: *date* (ignoring time), *hour* of the day and even the *day of the week*.

```python
# Parse timestamp 
ddf["ts"] = dd.to_datetime(ddf["ts"])

# Calculate feature columns
ddf["date"] = ddf["ts"].dt.date
ddf["hour"] = ddf["ts"].dt.hour
ddf["dayofweek"] = ddf["ts"].dt.dayofweek
```

On the other hand, the spatial component is a hurdle. The operation of determining whether a given point lies inside a boundary, or **point-in-polygon**, is computationally intensive.

**Here's the trick**. We calculate a *spatial index*!

A *geospatial index* divides areas of the Earth creating a grid system of determined grid cells. Then, the spatial information is translated into a hierarchical index, that can be easily joined relationally with other datasets.

In our example, we use [H3: Hexagonal Hierarchical Spatial Index](https://github.com/uber/h3) developed and open-sourced by [Uber](https://eng.uber.com/h3/). See below an illustration of bucketing coordinates onto H3 hexagons.

<figure align="center">
  <img src="waze-h3.png"/>
  <figcaption>The maps above depict the process of bucketing points with H3</figcaption>
  <figcaption>Source: https://eng.uber.com/h3</figcaption>
</figure>

And, finally, applying the [h3.geo_to_h3](https://h3geo.org/docs/api/indexing#geotoh3) function to the [Dask DataFrame](https://docs.dask.org/en/stable/dataframe.html), with zoom 10, or approximately **15000 m²**.

```python
ddf["h3"] = ddf.apply(lambda x: h3.geo_to_h3(x["lat"], x["lon"], 10), axis=1)
```

### Visualizing (Millions of) Waze Traffic Alerts

Now, with the geospatial index in hands, we are ready to reduce the order of millions to thousands without losing crucial insighs such as movements trends.

#### Aggregating on type, month and hour of the day

<div class="bk-root" id="56ac4419-8d86-49df-9fc6-e8f5f030306a" data-root-id="3798"></div>

<script type="application/json" id="3980">
  {"6e33217c-6369-4397-82bf-a6e3ff140cfb":{"defs":[],"roots":{"references":[{"attributes":{},"id":"3816","type":"PanTool"},{"attributes":{"axis_label":"Hour of the day","axis_label_text_font_size":"10pt","axis_line_color":null,"coordinates":null,"formatter":{"id":"3839"},"group":null,"major_label_policy":{"id":"3840"},"major_label_standoff":0,"major_label_text_font_size":"12pt","major_tick_line_color":null,"ticker":{"id":"3813"}},"id":"3812","type":"CategoricalAxis"},{"attributes":{},"id":"3839","type":"CategoricalTickFormatter"},{"attributes":{},"id":"3844","type":"AllLabels"},{"attributes":{"tools":[{"id":"3815"},{"id":"3816"},{"id":"3817"},{"id":"3818"},{"id":"3819"}]},"id":"3821","type":"Toolbar"},{"attributes":{"high":2115,"low":32,"palette":["#00204C","#00204E","#002150","#002251","#002353","#002355","#002456","#002558","#00265A","#00265B","#00275D","#00285F","#002861","#002963","#002A64","#002A66","#002B68","#002C6A","#002D6C","#002D6D","#002E6E","#002E6F","#002F6F","#002F6F","#00306F","#00316F","#00316F","#00326E","#00336E","#00346E","#00346E","#01356E","#06366E","#0A376D","#0E376D","#12386D","#15396D","#17396D","#1A3A6C","#1C3B6C","#1E3C6C","#203C6C","#223D6C","#243E6C","#263E6C","#273F6C","#29406B","#2B416B","#2C416B","#2E426B","#2F436B","#31446B","#32446B","#33456B","#35466B","#36466B","#37476B","#38486B","#3A496B","#3B496B","#3C4A6B","#3D4B6B","#3E4B6B","#404C6B","#414D6B","#424E6B","#434E6B","#444F6B","#45506B","#46506B","#47516B","#48526B","#49536B","#4A536B","#4B546B","#4C556B","#4D556B","#4E566B","#4F576C","#50586C","#51586C","#52596C","#535A6C","#545A6C","#555B6C","#565C6C","#575D6D","#585D6D","#595E6D","#5A5F6D","#5B5F6D","#5C606D","#5D616E","#5E626E","#5F626E","#5F636E","#60646E","#61656F","#62656F","#63666F","#64676F","#65676F","#666870","#676970","#686A70","#686A70","#696B71","#6A6C71","#6B6D71","#6C6D72","#6D6E72","#6E6F72","#6F6F72","#6F7073","#707173","#717273","#727274","#737374","#747475","#757575","#757575","#767676","#777776","#787876","#797877","#7A7977","#7B7A77","#7B7B78","#7C7B78","#7D7C78","#7E7D78","#7F7E78","#807E78","#817F78","#828078","#838178","#848178","#858278","#868378","#878478","#888578","#898578","#8A8678","#8B8778","#8C8878","#8D8878","#8E8978","#8F8A78","#908B78","#918C78","#928C78","#938D78","#948E78","#958F78","#968F77","#979077","#989177","#999277","#9A9377","#9B9377","#9C9477","#9D9577","#9E9676","#9F9776","#A09876","#A19876","#A29976","#A39A75","#A49B75","#A59C75","#A69C75","#A79D75","#A89E74","#A99F74","#AAA074","#ABA174","#ACA173","#ADA273","#AEA373","#AFA473","#B0A572","#B1A672","#B2A672","#B4A771","#B5A871","#B6A971","#B7AA70","#B8AB70","#B9AB70","#BAAC6F","#BBAD6F","#BCAE6E","#BDAF6E","#BEB06E","#BFB16D","#C0B16D","#C1B26C","#C2B36C","#C3B46C","#C5B56B","#C6B66B","#C7B76A","#C8B86A","#C9B869","#CAB969","#CBBA68","#CCBB68","#CDBC67","#CEBD67","#D0BE66","#D1BF66","#D2C065","#D3C065","#D4C164","#D5C263","#D6C363","#D7C462","#D8C561","#D9C661","#DBC760","#DCC860","#DDC95F","#DECA5E","#DFCB5D","#E0CB5D","#E1CC5C","#E3CD5B","#E4CE5B","#E5CF5A","#E6D059","#E7D158","#E8D257","#E9D356","#EBD456","#ECD555","#EDD654","#EED753","#EFD852","#F0D951","#F1DA50","#F3DB4F","#F4DC4E","#F5DD4D","#F6DE4C","#F7DF4B","#F9E049","#FAE048","#FBE147","#FCE246","#FDE345","#FFE443","#FFE542","#FFE642","#FFE743","#FFE844","#FFE945"]},"id":"3797","type":"LinearColorMapper"},{"attributes":{"above":[{"id":"3809"}],"center":[{"id":"3811"},{"id":"3814"}],"left":[{"id":"3812"}],"renderers":[{"id":"3832"}],"right":[{"id":"3836"}],"title":{"id":"3799"},"toolbar":{"id":"3821"},"width":800,"x_range":{"id":"3801"},"x_scale":{"id":"3805"},"y_range":{"id":"3803"},"y_scale":{"id":"3807"}},"id":"3798","subtype":"Figure","type":"Plot"},{"attributes":{"data":{"count":[520,666,523,632,532,443,522,579,558,370,735,660,454,469,682,418,481,469,454,455,509,471,264,366,440,384,374,267,327,248,272,180,216,377,309,278,156,112,166,282,113,257,158,223,227,235,206,176,163,75,169,105,71,108,93,123,158,115,125,155,83,43,116,128,78,79,78,110,60,63,62,45,48,55,42,68,35,71,93,53,32,58,124,63,75,60,80,62,67,99,64,44,112,41,67,86,164,219,251,190,192,273,212,185,303,134,272,256,648,748,748,597,441,765,474,565,793,506,733,569,807,676,1114,919,1079,1181,1050,1240,765,1050,1038,1168,975,1160,775,1275,764,1217,1152,1113,887,1257,1084,1280,1070,1029,1062,967,1030,778,901,722,688,1096,830,954,974,1011,949,1028,1046,1126,779,1083,1040,956,791,1010,983,776,1306,1092,925,1085,1016,1055,978,785,1060,986,1143,1139,1158,836,1275,913,1428,1056,1144,1003,1283,1245,990,909,1325,808,1497,1280,1092,1332,1247,1492,1387,1273,1406,1060,1360,817,1210,973,1345,1210,1654,1196,1127,1414,1473,1430,1285,1629,1475,1428,882,1273,1226,925,1399,1267,1696,1717,1074,1579,1625,1717,1393,1375,1456,1501,1162,1558,1986,1765,1810,1699,1254,1798,1733,1653,1271,1485,1397,1492,1808,1533,2059,2115,1428,1100,1395,2094,1825,1616,1665,1157,1388,881,1082,898,1332,1537,1525,1567,1288,1200,1605,1160,678,925,798,781,851,969,929,599,610,816,861,1047],"date":["Jan","May","Jun","Jul","Feb","Mar","Aug","Sep","Oct","Apr","Dec","Nov","Feb","May","Dec","Jan","Aug","Jun","Sep","Jul","Oct","Nov","Apr","Mar","Dec","Aug","Nov","Jun","Jul","May","Jan","Apr","Mar","Oct","Sep","Feb","May","Apr","Jun","Dec","Mar","Nov","Jan","Jul","Oct","Aug","Sep","Feb","Nov","Mar","Oct","Jan","Apr","May","Feb","Sep","Dec","Jun","Jul","Aug","May","Apr","Dec","Oct","Aug","Nov","Jul","Sep","Jun","Feb","Jan","Mar","Jan","May","Feb","Aug","Apr","Nov","Oct","Jun","Mar","Jul","Dec","Sep","Sep","May","Feb","Jul","Jan","Dec","Aug","Apr","Oct","Mar","Jun","Nov","Jan","Jun","Nov","Feb","May","Aug","Jul","Mar","Oct","Apr","Dec","Sep","Jun","Nov","Aug","Feb","Apr","Dec","Jan","Jul","Oct","May","Sep","Mar","Mar","Jan","Nov","Feb","Jul","Jun","Dec","Aug","Apr","Sep","May","Oct","Feb","May","Jan","Jul","Mar","Nov","Sep","Oct","Apr","Aug","Dec","Jun","Dec","May","Jul","Aug","Nov","Jan","Sep","Apr","Mar","Jun","Feb","Oct","Aug","Oct","Feb","Sep","Jul","Dec","Apr","Nov","Jun","Jan","Mar","May","Jun","Mar","Dec","Sep","Jan","Nov","Jul","Aug","Feb","Apr","Oct","May","May","Feb","Sep","Apr","Oct","Mar","Dec","Jun","Jul","Jan","Nov","Aug","Jan","Mar","Aug","Apr","Oct","May","Feb","Jul","Sep","Dec","Nov","Jun","Nov","Jan","Jul","Apr","Sep","Mar","Aug","May","Dec","Jun","Feb","Oct","Oct","Nov","Jan","Dec","Aug","Jul","Mar","Jun","Feb","Apr","May","Sep","Nov","Oct","Apr","Aug","Jul","Dec","Feb","Jan","Jun","Sep","Mar","May","Dec","May","Jul","Nov","Mar","Jun","Oct","Aug","Apr","Sep","Jan","Feb","Oct","Sep","Jun","May","Feb","Mar","Apr","Jul","Aug","Nov","Dec","Jan","Dec","Jan","Apr","Mar","Aug","Jun","Oct","Jul","Nov","Sep","May","Feb","Jan","Oct","Sep","Feb","May","Jul","Nov","Apr","Mar","Jun","Aug","Dec"],"hour":["0:00","0:00","0:00","0:00","0:00","0:00","0:00","0:00","0:00","0:00","0:00","0:00","1:00","1:00","1:00","1:00","1:00","1:00","1:00","1:00","1:00","1:00","1:00","1:00","2:00","2:00","2:00","2:00","2:00","2:00","2:00","2:00","2:00","2:00","2:00","2:00","3:00","3:00","3:00","3:00","3:00","3:00","3:00","3:00","3:00","3:00","3:00","3:00","4:00","4:00","4:00","4:00","4:00","4:00","4:00","4:00","4:00","4:00","4:00","4:00","5:00","5:00","5:00","5:00","5:00","5:00","5:00","5:00","5:00","5:00","5:00","5:00","6:00","6:00","6:00","6:00","6:00","6:00","6:00","6:00","6:00","6:00","6:00","6:00","7:00","7:00","7:00","7:00","7:00","7:00","7:00","7:00","7:00","7:00","7:00","7:00","8:00","8:00","8:00","8:00","8:00","8:00","8:00","8:00","8:00","8:00","8:00","8:00","9:00","9:00","9:00","9:00","9:00","9:00","9:00","9:00","9:00","9:00","9:00","9:00","10:00","10:00","10:00","10:00","10:00","10:00","10:00","10:00","10:00","10:00","10:00","10:00","11:00","11:00","11:00","11:00","11:00","11:00","11:00","11:00","11:00","11:00","11:00","11:00","12:00","12:00","12:00","12:00","12:00","12:00","12:00","12:00","12:00","12:00","12:00","12:00","13:00","13:00","13:00","13:00","13:00","13:00","13:00","13:00","13:00","13:00","13:00","13:00","14:00","14:00","14:00","14:00","14:00","14:00","14:00","14:00","14:00","14:00","14:00","14:00","15:00","15:00","15:00","15:00","15:00","15:00","15:00","15:00","15:00","15:00","15:00","15:00","16:00","16:00","16:00","16:00","16:00","16:00","16:00","16:00","16:00","16:00","16:00","16:00","17:00","17:00","17:00","17:00","17:00","17:00","17:00","17:00","17:00","17:00","17:00","17:00","18:00","18:00","18:00","18:00","18:00","18:00","18:00","18:00","18:00","18:00","18:00","18:00","19:00","19:00","19:00","19:00","19:00","19:00","19:00","19:00","19:00","19:00","19:00","19:00","20:00","20:00","20:00","20:00","20:00","20:00","20:00","20:00","20:00","20:00","20:00","20:00","21:00","21:00","21:00","21:00","21:00","21:00","21:00","21:00","21:00","21:00","21:00","21:00","22:00","22:00","22:00","22:00","22:00","22:00","22:00","22:00","22:00","22:00","22:00","22:00","23:00","23:00","23:00","23:00","23:00","23:00","23:00","23:00","23:00","23:00","23:00","23:00"],"index":[0,96,120,144,24,48,168,192,216,72,264,240,25,97,265,1,169,121,193,145,217,241,73,49,266,170,242,122,146,98,2,74,50,218,194,26,99,75,123,267,51,243,3,147,219,171,195,27,244,52,220,4,76,100,28,196,268,124,148,172,101,77,269,221,173,245,149,197,125,29,5,53,6,102,30,174,78,246,222,126,54,150,270,198,199,103,31,151,7,271,175,79,223,55,127,247,8,128,248,32,104,176,152,56,224,80,272,200,129,249,177,33,81,273,9,153,225,105,201,57,58,10,250,34,154,130,274,178,82,202,106,226,35,107,11,155,59,251,203,227,83,179,275,131,276,108,156,180,252,12,204,84,60,132,36,228,181,229,37,205,157,277,85,253,133,13,61,109,134,62,278,206,14,254,158,182,38,86,230,110,111,39,207,87,231,63,279,135,159,15,255,183,16,64,184,88,232,112,40,160,208,280,256,136,257,17,161,89,209,65,185,113,281,137,41,233,234,258,18,282,186,162,66,138,42,90,114,210,259,235,91,187,163,283,43,19,139,211,67,115,284,116,164,260,68,140,236,188,92,212,20,44,237,213,141,117,45,69,93,165,189,261,285,21,286,22,94,70,190,142,238,166,262,214,118,46,23,239,215,47,119,167,263,95,71,143,191,287],"month":[1,5,6,7,2,3,8,9,10,4,12,11,2,5,12,1,8,6,9,7,10,11,4,3,12,8,11,6,7,5,1,4,3,10,9,2,5,4,6,12,3,11,1,7,10,8,9,2,11,3,10,1,4,5,2,9,12,6,7,8,5,4,12,10,8,11,7,9,6,2,1,3,1,5,2,8,4,11,10,6,3,7,12,9,9,5,2,7,1,12,8,4,10,3,6,11,1,6,11,2,5,8,7,3,10,4,12,9,6,11,8,2,4,12,1,7,10,5,9,3,3,1,11,2,7,6,12,8,4,9,5,10,2,5,1,7,3,11,9,10,4,8,12,6,12,5,7,8,11,1,9,4,3,6,2,10,8,10,2,9,7,12,4,11,6,1,3,5,6,3,12,9,1,11,7,8,2,4,10,5,5,2,9,4,10,3,12,6,7,1,11,8,1,3,8,4,10,5,2,7,9,12,11,6,11,1,7,4,9,3,8,5,12,6,2,10,10,11,1,12,8,7,3,6,2,4,5,9,11,10,4,8,7,12,2,1,6,9,3,5,12,5,7,11,3,6,10,8,4,9,1,2,10,9,6,5,2,3,4,7,8,11,12,1,12,1,4,3,8,6,10,7,11,9,5,2,1,10,9,2,5,7,11,4,3,6,8,12],"month_name":["Jan","May","Jun","Jul","Feb","Mar","Aug","Sep","Oct","Apr","Dec","Nov","Feb","May","Dec","Jan","Aug","Jun","Sep","Jul","Oct","Nov","Apr","Mar","Dec","Aug","Nov","Jun","Jul","May","Jan","Apr","Mar","Oct","Sep","Feb","May","Apr","Jun","Dec","Mar","Nov","Jan","Jul","Oct","Aug","Sep","Feb","Nov","Mar","Oct","Jan","Apr","May","Feb","Sep","Dec","Jun","Jul","Aug","May","Apr","Dec","Oct","Aug","Nov","Jul","Sep","Jun","Feb","Jan","Mar","Jan","May","Feb","Aug","Apr","Nov","Oct","Jun","Mar","Jul","Dec","Sep","Sep","May","Feb","Jul","Jan","Dec","Aug","Apr","Oct","Mar","Jun","Nov","Jan","Jun","Nov","Feb","May","Aug","Jul","Mar","Oct","Apr","Dec","Sep","Jun","Nov","Aug","Feb","Apr","Dec","Jan","Jul","Oct","May","Sep","Mar","Mar","Jan","Nov","Feb","Jul","Jun","Dec","Aug","Apr","Sep","May","Oct","Feb","May","Jan","Jul","Mar","Nov","Sep","Oct","Apr","Aug","Dec","Jun","Dec","May","Jul","Aug","Nov","Jan","Sep","Apr","Mar","Jun","Feb","Oct","Aug","Oct","Feb","Sep","Jul","Dec","Apr","Nov","Jun","Jan","Mar","May","Jun","Mar","Dec","Sep","Jan","Nov","Jul","Aug","Feb","Apr","Oct","May","May","Feb","Sep","Apr","Oct","Mar","Dec","Jun","Jul","Jan","Nov","Aug","Jan","Mar","Aug","Apr","Oct","May","Feb","Jul","Sep","Dec","Nov","Jun","Nov","Jan","Jul","Apr","Sep","Mar","Aug","May","Dec","Jun","Feb","Oct","Oct","Nov","Jan","Dec","Aug","Jul","Mar","Jun","Feb","Apr","May","Sep","Nov","Oct","Apr","Aug","Jul","Dec","Feb","Jan","Jun","Sep","Mar","May","Dec","May","Jul","Nov","Mar","Jun","Oct","Aug","Apr","Sep","Jan","Feb","Oct","Sep","Jun","May","Feb","Mar","Apr","Jul","Aug","Nov","Dec","Jan","Dec","Jan","Apr","Mar","Aug","Jun","Oct","Jul","Nov","Sep","May","Feb","Jan","Oct","Sep","Feb","May","Jul","Nov","Apr","Mar","Jun","Aug","Dec"]},"selected":{"id":"3846"},"selection_policy":{"id":"3845"}},"id":"3827","type":"ColumnDataSource"},{"attributes":{"source":{"id":"3827"}},"id":"3833","type":"CDSView"},{"attributes":{"factors":["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"]},"id":"3801","type":"FactorRange"},{"attributes":{},"id":"3807","type":"CategoricalScale"},{"attributes":{"callback":null,"tooltips":[["month","@month_name"],["count","@{count}{0,000f}"]]},"id":"3815","type":"HoverTool"},{"attributes":{},"id":"3810","type":"CategoricalTicker"},{"attributes":{},"id":"3805","type":"CategoricalScale"},{"attributes":{},"id":"3813","type":"CategoricalTicker"},{"attributes":{},"id":"3841","type":"NoOverlap"},{"attributes":{"desired_num_ticks":10},"id":"3834","type":"BasicTicker"},{"attributes":{},"id":"3819","type":"WheelZoomTool"},{"attributes":{},"id":"3818","type":"ResetTool"},{"attributes":{"color_mapper":{"id":"3797"},"coordinates":null,"formatter":{"id":"3835"},"group":null,"label_standoff":6,"location":[0,0],"major_label_policy":{"id":"3841"},"major_label_text_font_size":"5pt","ticker":{"id":"3834"}},"id":"3836","type":"ColorBar"},{"attributes":{"axis":{"id":"3812"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"3814","type":"Grid"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"field":"count","transform":{"id":"3797"}},"hatch_alpha":{"value":0.1},"height":{"value":1},"line_alpha":{"value":0.1},"line_color":{"value":null},"width":{"value":1},"x":{"field":"date"},"y":{"field":"hour"}},"id":"3830","type":"Rect"},{"attributes":{"overlay":{"id":"3820"}},"id":"3817","type":"BoxZoomTool"},{"attributes":{},"id":"3840","type":"AllLabels"},{"attributes":{},"id":"3846","type":"Selection"},{"attributes":{"axis":{"id":"3809"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"3811","type":"Grid"},{"attributes":{"coordinates":null,"group":null,"text":"Waze alerts (ACCIDENT) aggregated by month and hour of day for S\u00e3o Paulo in 2021","text_font_size":"12pt"},"id":"3799","type":"Title"},{"attributes":{"factors":["23:00","22:00","21:00","20:00","19:00","18:00","17:00","16:00","15:00","14:00","13:00","12:00","11:00","10:00","9:00","8:00","7:00","6:00","5:00","4:00","3:00","2:00","1:00","0:00"]},"id":"3803","type":"FactorRange"},{"attributes":{"axis_label":"Month","axis_label_text_font_size":"10pt","axis_line_color":null,"coordinates":null,"formatter":{"id":"3843"},"group":null,"major_label_orientation":1.0471975511965976,"major_label_policy":{"id":"3844"},"major_label_standoff":0,"major_label_text_font_size":"12pt","major_tick_line_color":null,"ticker":{"id":"3810"}},"id":"3809","type":"CategoricalAxis"},{"attributes":{},"id":"3843","type":"CategoricalTickFormatter"},{"attributes":{"coordinates":null,"data_source":{"id":"3827"},"glyph":{"id":"3829"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"3831"},"nonselection_glyph":{"id":"3830"},"view":{"id":"3833"}},"id":"3832","type":"GlyphRenderer"},{"attributes":{"fill_color":{"field":"count","transform":{"id":"3797"}},"height":{"value":1},"line_color":{"value":null},"width":{"value":1},"x":{"field":"date"},"y":{"field":"hour"}},"id":"3829","type":"Rect"},{"attributes":{},"id":"3845","type":"UnionRenderers"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"field":"count","transform":{"id":"3797"}},"hatch_alpha":{"value":0.2},"height":{"value":1},"line_alpha":{"value":0.2},"line_color":{"value":null},"width":{"value":1},"x":{"field":"date"},"y":{"field":"hour"}},"id":"3831","type":"Rect"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"3820","type":"BoxAnnotation"},{"attributes":{"format":"%d"},"id":"3835","type":"PrintfTickFormatter"}],"root_ids":["3798"]},"title":"Bokeh Application","version":"2.4.2"}}
</script>
<script type="text/javascript">
  (function() {
    const fn = function() {
      Bokeh.safely(function() {
        (function(root) {
          function embed_document(root) {

          const docs_json = document.getElementById('3980').textContent;
          const render_items = [{"docid":"6e33217c-6369-4397-82bf-a6e3ff140cfb","root_ids":["3798"],"roots":{"3798":"56ac4419-8d86-49df-9fc6-e8f5f030306a"}}];
          root.Bokeh.embed.embed_items(docs_json, render_items);

          }
          if (root.Bokeh !== undefined) {
            embed_document(root);
          } else {
            let attempts = 0;
            const timer = setInterval(function(root) {
              if (root.Bokeh !== undefined) {
                clearInterval(timer);
                embed_document(root);
              } else {
                attempts++;
                if (attempts > 100) {
                  clearInterval(timer);
                  console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
                }
              }
            }, 10, root)
          }
        })(window);
      });
    };
    if (document.readyState != "loading") fn();
    else document.addEventListener("DOMContentLoaded", fn);
  })();
</script>
<figcaption>Waze Traffic Alerts for São Paulo, Brazil in 2021. Data provided by Waze.</figcaption>

<div class="bk-root" id="828b10f3-e2c1-438c-a2df-5fcdf8d63973" data-root-id="3614"></div>

<script type="application/json" id="3796">
  {"f3a215d8-0018-4cf1-8e4e-15431a95d11c":{"defs":[],"roots":{"references":[{"attributes":{"desired_num_ticks":10},"id":"3650","type":"BasicTicker"},{"attributes":{"coordinates":null,"data_source":{"id":"3643"},"glyph":{"id":"3645"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"3647"},"nonselection_glyph":{"id":"3646"},"view":{"id":"3649"}},"id":"3648","type":"GlyphRenderer"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"field":"count","transform":{"id":"3613"}},"hatch_alpha":{"value":0.1},"height":{"value":1},"line_alpha":{"value":0.1},"line_color":{"value":null},"width":{"value":1},"x":{"field":"date"},"y":{"field":"hour"}},"id":"3646","type":"Rect"},{"attributes":{},"id":"3635","type":"WheelZoomTool"},{"attributes":{"data":{"count":[2871,2807,2756,3338,3267,1733,3334,3349,3984,1055,7168,5010,1810,990,4310,1505,1654,1197,1722,1436,2203,2680,481,707,2313,880,1566,511,618,449,743,321,390,1104,869,675,312,201,307,1071,249,891,375,294,479,587,489,385,383,127,356,257,160,324,264,311,569,306,288,337,218,110,333,196,201,264,201,268,154,170,175,115,64,135,93,107,50,116,94,101,63,94,192,162,210,252,342,293,285,230,291,177,243,326,265,283,910,1083,1276,1087,1105,1547,1063,891,1227,658,1450,1302,11013,17450,17490,13022,6626,12482,7928,8680,15246,10182,15471,8404,14553,13946,35502,24400,21728,27524,25165,33144,16718,30072,27231,29654,24357,30601,16541,29362,13031,36328,28213,30379,17292,30168,32285,31554,28774,24639,26021,21954,27726,14954,20998,14146,10827,25645,20269,22871,24481,25203,23205,24467,22401,31837,13400,27881,21178,19265,13690,20791,20414,12176,32962,22909,17506,26191,21898,23623,22137,12100,25028,20699,24507,24395,28994,12729,33424,12006,38900,23834,23592,16862,35846,31886,15760,11951,31834,12361,35252,23778,24416,25374,28461,37202,36057,25573,30378,17187,27380,12568,24266,12487,27381,22327,34526,24007,23076,29469,32354,33238,19662,37764,30662,30517,13923,26978,26038,14764,25198,27808,43639,43559,19469,42499,37227,46900,32858,25158,33412,37215,19345,31946,49438,51068,51370,47703,22312,50905,47866,48522,29093,40930,24505,38275,50142,43295,56293,57960,38756,22512,35424,56908,52309,48593,48845,22300,31740,10783,16495,9388,22067,24933,25644,28423,27842,19717,25871,19916,5150,9711,7128,7849,7028,8046,12465,3825,3540,6027,7438,16110],"date":["Jan","May","Jun","Jul","Feb","Mar","Aug","Sep","Oct","Apr","Dec","Nov","Feb","May","Dec","Jan","Aug","Jun","Sep","Jul","Oct","Nov","Apr","Mar","Dec","Aug","Nov","Jun","Jul","May","Jan","Apr","Mar","Oct","Sep","Feb","May","Apr","Jun","Dec","Mar","Nov","Jan","Jul","Oct","Aug","Sep","Feb","Nov","Mar","Oct","Jan","Apr","May","Feb","Sep","Dec","Jun","Jul","Aug","May","Apr","Dec","Oct","Aug","Nov","Jul","Sep","Jun","Feb","Jan","Mar","Jan","May","Feb","Aug","Apr","Nov","Oct","Jun","Mar","Jul","Dec","Sep","Sep","May","Feb","Jul","Jan","Dec","Aug","Apr","Oct","Mar","Jun","Nov","Jan","Jun","Nov","Feb","May","Aug","Jul","Mar","Oct","Apr","Dec","Sep","Jun","Nov","Aug","Feb","Apr","Dec","Jan","Jul","Oct","May","Sep","Mar","Mar","Jan","Nov","Feb","Jul","Jun","Dec","Aug","Apr","Sep","May","Oct","Feb","May","Jan","Jul","Mar","Nov","Sep","Oct","Apr","Aug","Dec","Jun","Dec","May","Jul","Aug","Nov","Jan","Sep","Apr","Mar","Jun","Feb","Oct","Aug","Oct","Feb","Sep","Jul","Dec","Apr","Nov","Jun","Jan","Mar","May","Jun","Mar","Dec","Sep","Jan","Nov","Jul","Aug","Feb","Apr","Oct","May","May","Feb","Sep","Apr","Oct","Mar","Dec","Jun","Jul","Jan","Nov","Aug","Jan","Mar","Aug","Apr","Oct","May","Feb","Jul","Sep","Dec","Nov","Jun","Nov","Jan","Jul","Apr","Sep","Mar","Aug","May","Dec","Jun","Feb","Oct","Oct","Nov","Jan","Dec","Aug","Jul","Mar","Jun","Feb","Apr","May","Sep","Nov","Oct","Apr","Aug","Jul","Dec","Feb","Jan","Jun","Sep","Mar","May","Dec","May","Jul","Nov","Mar","Jun","Oct","Aug","Apr","Sep","Jan","Feb","Oct","Sep","Jun","May","Feb","Mar","Apr","Jul","Aug","Nov","Dec","Jan","Dec","Jan","Apr","Mar","Aug","Jun","Oct","Jul","Nov","Sep","May","Feb","Jan","Oct","Sep","Feb","May","Jul","Nov","Apr","Mar","Jun","Aug","Dec"],"hour":["0:00","0:00","0:00","0:00","0:00","0:00","0:00","0:00","0:00","0:00","0:00","0:00","1:00","1:00","1:00","1:00","1:00","1:00","1:00","1:00","1:00","1:00","1:00","1:00","2:00","2:00","2:00","2:00","2:00","2:00","2:00","2:00","2:00","2:00","2:00","2:00","3:00","3:00","3:00","3:00","3:00","3:00","3:00","3:00","3:00","3:00","3:00","3:00","4:00","4:00","4:00","4:00","4:00","4:00","4:00","4:00","4:00","4:00","4:00","4:00","5:00","5:00","5:00","5:00","5:00","5:00","5:00","5:00","5:00","5:00","5:00","5:00","6:00","6:00","6:00","6:00","6:00","6:00","6:00","6:00","6:00","6:00","6:00","6:00","7:00","7:00","7:00","7:00","7:00","7:00","7:00","7:00","7:00","7:00","7:00","7:00","8:00","8:00","8:00","8:00","8:00","8:00","8:00","8:00","8:00","8:00","8:00","8:00","9:00","9:00","9:00","9:00","9:00","9:00","9:00","9:00","9:00","9:00","9:00","9:00","10:00","10:00","10:00","10:00","10:00","10:00","10:00","10:00","10:00","10:00","10:00","10:00","11:00","11:00","11:00","11:00","11:00","11:00","11:00","11:00","11:00","11:00","11:00","11:00","12:00","12:00","12:00","12:00","12:00","12:00","12:00","12:00","12:00","12:00","12:00","12:00","13:00","13:00","13:00","13:00","13:00","13:00","13:00","13:00","13:00","13:00","13:00","13:00","14:00","14:00","14:00","14:00","14:00","14:00","14:00","14:00","14:00","14:00","14:00","14:00","15:00","15:00","15:00","15:00","15:00","15:00","15:00","15:00","15:00","15:00","15:00","15:00","16:00","16:00","16:00","16:00","16:00","16:00","16:00","16:00","16:00","16:00","16:00","16:00","17:00","17:00","17:00","17:00","17:00","17:00","17:00","17:00","17:00","17:00","17:00","17:00","18:00","18:00","18:00","18:00","18:00","18:00","18:00","18:00","18:00","18:00","18:00","18:00","19:00","19:00","19:00","19:00","19:00","19:00","19:00","19:00","19:00","19:00","19:00","19:00","20:00","20:00","20:00","20:00","20:00","20:00","20:00","20:00","20:00","20:00","20:00","20:00","21:00","21:00","21:00","21:00","21:00","21:00","21:00","21:00","21:00","21:00","21:00","21:00","22:00","22:00","22:00","22:00","22:00","22:00","22:00","22:00","22:00","22:00","22:00","22:00","23:00","23:00","23:00","23:00","23:00","23:00","23:00","23:00","23:00","23:00","23:00","23:00"],"index":[0,96,120,144,24,48,168,192,216,72,264,240,25,97,265,1,169,121,193,145,217,241,73,49,266,170,242,122,146,98,2,74,50,218,194,26,99,75,123,267,51,243,3,147,219,171,195,27,244,52,220,4,76,100,28,196,268,124,148,172,101,77,269,221,173,245,149,197,125,29,5,53,6,102,30,174,78,246,222,126,54,150,270,198,199,103,31,151,7,271,175,79,223,55,127,247,8,128,248,32,104,176,152,56,224,80,272,200,129,249,177,33,81,273,9,153,225,105,201,57,58,10,250,34,154,130,274,178,82,202,106,226,35,107,11,155,59,251,203,227,83,179,275,131,276,108,156,180,252,12,204,84,60,132,36,228,181,229,37,205,157,277,85,253,133,13,61,109,134,62,278,206,14,254,158,182,38,86,230,110,111,39,207,87,231,63,279,135,159,15,255,183,16,64,184,88,232,112,40,160,208,280,256,136,257,17,161,89,209,65,185,113,281,137,41,233,234,258,18,282,186,162,66,138,42,90,114,210,259,235,91,187,163,283,43,19,139,211,67,115,284,116,164,260,68,140,236,188,92,212,20,44,237,213,141,117,45,69,93,165,189,261,285,21,286,22,94,70,190,142,238,166,262,214,118,46,23,239,215,47,119,167,263,95,71,143,191,287],"month":[1,5,6,7,2,3,8,9,10,4,12,11,2,5,12,1,8,6,9,7,10,11,4,3,12,8,11,6,7,5,1,4,3,10,9,2,5,4,6,12,3,11,1,7,10,8,9,2,11,3,10,1,4,5,2,9,12,6,7,8,5,4,12,10,8,11,7,9,6,2,1,3,1,5,2,8,4,11,10,6,3,7,12,9,9,5,2,7,1,12,8,4,10,3,6,11,1,6,11,2,5,8,7,3,10,4,12,9,6,11,8,2,4,12,1,7,10,5,9,3,3,1,11,2,7,6,12,8,4,9,5,10,2,5,1,7,3,11,9,10,4,8,12,6,12,5,7,8,11,1,9,4,3,6,2,10,8,10,2,9,7,12,4,11,6,1,3,5,6,3,12,9,1,11,7,8,2,4,10,5,5,2,9,4,10,3,12,6,7,1,11,8,1,3,8,4,10,5,2,7,9,12,11,6,11,1,7,4,9,3,8,5,12,6,2,10,10,11,1,12,8,7,3,6,2,4,5,9,11,10,4,8,7,12,2,1,6,9,3,5,12,5,7,11,3,6,10,8,4,9,1,2,10,9,6,5,2,3,4,7,8,11,12,1,12,1,4,3,8,6,10,7,11,9,5,2,1,10,9,2,5,7,11,4,3,6,8,12],"month_name":["Jan","May","Jun","Jul","Feb","Mar","Aug","Sep","Oct","Apr","Dec","Nov","Feb","May","Dec","Jan","Aug","Jun","Sep","Jul","Oct","Nov","Apr","Mar","Dec","Aug","Nov","Jun","Jul","May","Jan","Apr","Mar","Oct","Sep","Feb","May","Apr","Jun","Dec","Mar","Nov","Jan","Jul","Oct","Aug","Sep","Feb","Nov","Mar","Oct","Jan","Apr","May","Feb","Sep","Dec","Jun","Jul","Aug","May","Apr","Dec","Oct","Aug","Nov","Jul","Sep","Jun","Feb","Jan","Mar","Jan","May","Feb","Aug","Apr","Nov","Oct","Jun","Mar","Jul","Dec","Sep","Sep","May","Feb","Jul","Jan","Dec","Aug","Apr","Oct","Mar","Jun","Nov","Jan","Jun","Nov","Feb","May","Aug","Jul","Mar","Oct","Apr","Dec","Sep","Jun","Nov","Aug","Feb","Apr","Dec","Jan","Jul","Oct","May","Sep","Mar","Mar","Jan","Nov","Feb","Jul","Jun","Dec","Aug","Apr","Sep","May","Oct","Feb","May","Jan","Jul","Mar","Nov","Sep","Oct","Apr","Aug","Dec","Jun","Dec","May","Jul","Aug","Nov","Jan","Sep","Apr","Mar","Jun","Feb","Oct","Aug","Oct","Feb","Sep","Jul","Dec","Apr","Nov","Jun","Jan","Mar","May","Jun","Mar","Dec","Sep","Jan","Nov","Jul","Aug","Feb","Apr","Oct","May","May","Feb","Sep","Apr","Oct","Mar","Dec","Jun","Jul","Jan","Nov","Aug","Jan","Mar","Aug","Apr","Oct","May","Feb","Jul","Sep","Dec","Nov","Jun","Nov","Jan","Jul","Apr","Sep","Mar","Aug","May","Dec","Jun","Feb","Oct","Oct","Nov","Jan","Dec","Aug","Jul","Mar","Jun","Feb","Apr","May","Sep","Nov","Oct","Apr","Aug","Jul","Dec","Feb","Jan","Jun","Sep","Mar","May","Dec","May","Jul","Nov","Mar","Jun","Oct","Aug","Apr","Sep","Jan","Feb","Oct","Sep","Jun","May","Feb","Mar","Apr","Jul","Aug","Nov","Dec","Jan","Dec","Jan","Apr","Mar","Aug","Jun","Oct","Jul","Nov","Sep","May","Feb","Jan","Oct","Sep","Feb","May","Jul","Nov","Apr","Mar","Jun","Aug","Dec"]},"selected":{"id":"3662"},"selection_policy":{"id":"3661"}},"id":"3643","type":"ColumnDataSource"},{"attributes":{"fill_color":{"field":"count","transform":{"id":"3613"}},"height":{"value":1},"line_color":{"value":null},"width":{"value":1},"x":{"field":"date"},"y":{"field":"hour"}},"id":"3645","type":"Rect"},{"attributes":{"factors":["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"]},"id":"3617","type":"FactorRange"},{"attributes":{"axis_label":"Hour of the day","axis_label_text_font_size":"10pt","axis_line_color":null,"coordinates":null,"formatter":{"id":"3655"},"group":null,"major_label_policy":{"id":"3656"},"major_label_standoff":0,"major_label_text_font_size":"12pt","major_tick_line_color":null,"ticker":{"id":"3629"}},"id":"3628","type":"CategoricalAxis"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"3636","type":"BoxAnnotation"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"field":"count","transform":{"id":"3613"}},"hatch_alpha":{"value":0.2},"height":{"value":1},"line_alpha":{"value":0.2},"line_color":{"value":null},"width":{"value":1},"x":{"field":"date"},"y":{"field":"hour"}},"id":"3647","type":"Rect"},{"attributes":{"overlay":{"id":"3636"}},"id":"3633","type":"BoxZoomTool"},{"attributes":{},"id":"3662","type":"Selection"},{"attributes":{},"id":"3634","type":"ResetTool"},{"attributes":{"source":{"id":"3643"}},"id":"3649","type":"CDSView"},{"attributes":{},"id":"3626","type":"CategoricalTicker"},{"attributes":{},"id":"3621","type":"CategoricalScale"},{"attributes":{},"id":"3659","type":"CategoricalTickFormatter"},{"attributes":{"axis":{"id":"3628"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"3630","type":"Grid"},{"attributes":{"axis_label":"Month","axis_label_text_font_size":"10pt","axis_line_color":null,"coordinates":null,"formatter":{"id":"3659"},"group":null,"major_label_orientation":1.0471975511965976,"major_label_policy":{"id":"3660"},"major_label_standoff":0,"major_label_text_font_size":"12pt","major_tick_line_color":null,"ticker":{"id":"3626"}},"id":"3625","type":"CategoricalAxis"},{"attributes":{"coordinates":null,"group":null,"text":"Waze alerts (JAM) aggregated on month and hour of day for S\u00e3o Paulo, Brazil in 2021","text_font_size":"12pt"},"id":"3615","type":"Title"},{"attributes":{"factors":["23:00","22:00","21:00","20:00","19:00","18:00","17:00","16:00","15:00","14:00","13:00","12:00","11:00","10:00","9:00","8:00","7:00","6:00","5:00","4:00","3:00","2:00","1:00","0:00"]},"id":"3619","type":"FactorRange"},{"attributes":{"above":[{"id":"3625"}],"center":[{"id":"3627"},{"id":"3630"}],"left":[{"id":"3628"}],"renderers":[{"id":"3648"}],"right":[{"id":"3652"}],"title":{"id":"3615"},"toolbar":{"id":"3637"},"width":800,"x_range":{"id":"3617"},"x_scale":{"id":"3621"},"y_range":{"id":"3619"},"y_scale":{"id":"3623"}},"id":"3614","subtype":"Figure","type":"Plot"},{"attributes":{"color_mapper":{"id":"3613"},"coordinates":null,"formatter":{"id":"3651"},"group":null,"label_standoff":6,"location":[0,0],"major_label_policy":{"id":"3657"},"major_label_text_font_size":"5pt","ticker":{"id":"3650"}},"id":"3652","type":"ColorBar"},{"attributes":{},"id":"3656","type":"AllLabels"},{"attributes":{"tools":[{"id":"3631"},{"id":"3632"},{"id":"3633"},{"id":"3634"},{"id":"3635"}]},"id":"3637","type":"Toolbar"},{"attributes":{},"id":"3629","type":"CategoricalTicker"},{"attributes":{},"id":"3632","type":"PanTool"},{"attributes":{"high":57960,"low":50,"palette":["#00204C","#00204E","#002150","#002251","#002353","#002355","#002456","#002558","#00265A","#00265B","#00275D","#00285F","#002861","#002963","#002A64","#002A66","#002B68","#002C6A","#002D6C","#002D6D","#002E6E","#002E6F","#002F6F","#002F6F","#00306F","#00316F","#00316F","#00326E","#00336E","#00346E","#00346E","#01356E","#06366E","#0A376D","#0E376D","#12386D","#15396D","#17396D","#1A3A6C","#1C3B6C","#1E3C6C","#203C6C","#223D6C","#243E6C","#263E6C","#273F6C","#29406B","#2B416B","#2C416B","#2E426B","#2F436B","#31446B","#32446B","#33456B","#35466B","#36466B","#37476B","#38486B","#3A496B","#3B496B","#3C4A6B","#3D4B6B","#3E4B6B","#404C6B","#414D6B","#424E6B","#434E6B","#444F6B","#45506B","#46506B","#47516B","#48526B","#49536B","#4A536B","#4B546B","#4C556B","#4D556B","#4E566B","#4F576C","#50586C","#51586C","#52596C","#535A6C","#545A6C","#555B6C","#565C6C","#575D6D","#585D6D","#595E6D","#5A5F6D","#5B5F6D","#5C606D","#5D616E","#5E626E","#5F626E","#5F636E","#60646E","#61656F","#62656F","#63666F","#64676F","#65676F","#666870","#676970","#686A70","#686A70","#696B71","#6A6C71","#6B6D71","#6C6D72","#6D6E72","#6E6F72","#6F6F72","#6F7073","#707173","#717273","#727274","#737374","#747475","#757575","#757575","#767676","#777776","#787876","#797877","#7A7977","#7B7A77","#7B7B78","#7C7B78","#7D7C78","#7E7D78","#7F7E78","#807E78","#817F78","#828078","#838178","#848178","#858278","#868378","#878478","#888578","#898578","#8A8678","#8B8778","#8C8878","#8D8878","#8E8978","#8F8A78","#908B78","#918C78","#928C78","#938D78","#948E78","#958F78","#968F77","#979077","#989177","#999277","#9A9377","#9B9377","#9C9477","#9D9577","#9E9676","#9F9776","#A09876","#A19876","#A29976","#A39A75","#A49B75","#A59C75","#A69C75","#A79D75","#A89E74","#A99F74","#AAA074","#ABA174","#ACA173","#ADA273","#AEA373","#AFA473","#B0A572","#B1A672","#B2A672","#B4A771","#B5A871","#B6A971","#B7AA70","#B8AB70","#B9AB70","#BAAC6F","#BBAD6F","#BCAE6E","#BDAF6E","#BEB06E","#BFB16D","#C0B16D","#C1B26C","#C2B36C","#C3B46C","#C5B56B","#C6B66B","#C7B76A","#C8B86A","#C9B869","#CAB969","#CBBA68","#CCBB68","#CDBC67","#CEBD67","#D0BE66","#D1BF66","#D2C065","#D3C065","#D4C164","#D5C263","#D6C363","#D7C462","#D8C561","#D9C661","#DBC760","#DCC860","#DDC95F","#DECA5E","#DFCB5D","#E0CB5D","#E1CC5C","#E3CD5B","#E4CE5B","#E5CF5A","#E6D059","#E7D158","#E8D257","#E9D356","#EBD456","#ECD555","#EDD654","#EED753","#EFD852","#F0D951","#F1DA50","#F3DB4F","#F4DC4E","#F5DD4D","#F6DE4C","#F7DF4B","#F9E049","#FAE048","#FBE147","#FCE246","#FDE345","#FFE443","#FFE542","#FFE642","#FFE743","#FFE844","#FFE945"]},"id":"3613","type":"LinearColorMapper"},{"attributes":{},"id":"3661","type":"UnionRenderers"},{"attributes":{"format":"%d"},"id":"3651","type":"PrintfTickFormatter"},{"attributes":{"axis":{"id":"3625"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"3627","type":"Grid"},{"attributes":{"callback":null,"tooltips":[["month","@month_name"],["count","@{count}{0,000f}"]]},"id":"3631","type":"HoverTool"},{"attributes":{},"id":"3655","type":"CategoricalTickFormatter"},{"attributes":{},"id":"3660","type":"AllLabels"},{"attributes":{},"id":"3657","type":"NoOverlap"},{"attributes":{},"id":"3623","type":"CategoricalScale"}],"root_ids":["3614"]},"title":"Bokeh Application","version":"2.4.2"}}
</script>
<script type="text/javascript">
  (function() {
    const fn = function() {
      Bokeh.safely(function() {
        (function(root) {
          function embed_document(root) {

          const docs_json = document.getElementById('3796').textContent;
          const render_items = [{"docid":"f3a215d8-0018-4cf1-8e4e-15431a95d11c","root_ids":["3614"],"roots":{"3614":"828b10f3-e2c1-438c-a2df-5fcdf8d63973"}}];
          root.Bokeh.embed.embed_items(docs_json, render_items);

          }
          if (root.Bokeh !== undefined) {
            embed_document(root);
          } else {
            let attempts = 0;
            const timer = setInterval(function(root) {
              if (root.Bokeh !== undefined) {
                clearInterval(timer);
                embed_document(root);
              } else {
                attempts++;
                if (attempts > 100) {
                  clearInterval(timer);
                  console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
                }
              }
            }, 10, root)
          }
        })(window);
      });
    };
    if (document.readyState != "loading") fn();
    else document.addEventListener("DOMContentLoaded", fn);
  })();
</script>
<figcaption>Waze Traffic Alerts for São Paulo, Brazil in 2021. Data provided by Waze.</figcaption>

<br>

#### Aggregating on H3

Below the resulting map of more than **5 million Waze Traffic Alerts** in just a few minutes.

<style>
  #deck-map-container {
    width: 100%;
    height: 100%;
    background-color: black;
  }
  #map {
    pointer-events: none;
    height: 100%;
    width: 100%;
    position: absolute;
    z-index: 1;
  }
  #deckgl-overlay {
    z-index: 2;
    background: none;
  }
  #deck-map-wrapper {
    width: 100%;
    height: 100%;
  }
  #deck-container {
    width: 55vw;
    height: 70vh;
  }
</style>
<div id="deck-container"></div>

<script>
  const tooltip = {'text': 'Count: {count}'};
  const customLibraries = null;

  const deckInstance = createDeck({
          mapboxApiKey: 'pk.eyJ1IjoiZGF0YXBhcnRuZXJzaGlwIiwiYSI6ImNrcW1ocGw0MzBkdTIyc3BxeXAxNXZlbHYifQ.ZdJAt8Nd60sfW1vz0H07Ig',
                container: document.getElementById('deck-container'),
    jsonInput,
    tooltip,
    customLibraries
  });
</script>
<figcaption>Waze Traffic Alerts aggregated on H3 zoom 10 for São Paulo, Brazil in 2021. Data provided by Waze.</figcaption>

<br>
<br>

*To access Waze for Cities, see the [documentation](https://docs.datapartnership.org/partners/waze/README.html) and [submit a proposal](https://datapartnership.org/) through the Development Data Partnership Portal.*

*All examples and notebooks are available to the **Partnership's community** on the [GitHub repository](https://github.com/datapartnership/docs-waze) (private). If you haven't already and are staff of a participating organization, [join us on GitHub](https://datapartnership.org/join/)!*