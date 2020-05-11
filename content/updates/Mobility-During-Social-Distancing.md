+++
date = 2020-05-12T20:12:14Z
title = "Understanding mobility during social distancing with private sector data"
type = "Article"
slug = "understanding-mobility-during-social-distancing"
+++

As countries around the world work to contain the spread and impact of COVID-19, the World Bank Group is moving quickly to [provide fast, flexible responses](https://www.worldbank.org/en/news/factsheet/2020/02/11/how-the-world-bank-group-is-helping-countries-with-covid-19-coronavirus) to help developing countries strengthen their pandemic response and health care systems.

Part of this response is supporting countries' lock-down policies and evaluating their impacts. As governments strive to prevent the spread of COVID-19, they are also trying to prevent disproportionate socioeconomic impact in vulnerable communities. 

Understanding mobility across different communities, how it's influenced by policies, and identifying early potential hot spots is crucial to governments as they monitor and lift lockdowns.

## Using private sector data to understand mobility

Private sector data is crucial in helping governments measure how their policies impact human movement within different communities. 

For example, in areas with a higher median income and a more formal workforce, social distancing measures will likely have a higher degree of adherence. 

However, areas with lower income and a more informal workforce will have a harder time following social distancing guidelines. 

Several studies of United States counties support this hypothesis:

#### NYT
![](/updates/mobility-metrics-in/NYT.png)

*[NYT](https://www.nytimes.com/interactive/2020/04/03/us/coronavirus-stay-home-rich-poor.html) with [Cuebiq](https://www.cuebiq.com/) data: Census tract median of intra-day max extent of devices. 10% and 90% wealth percentiles. Wealthy counties esentially halted all movement and worked from home, while poorest counties trailed almost a week later, and with significant residual mobility as people could not afford to stay home.*

#### Reuters
![](/updates/mobility-metrics-in/Reuters.png)
*[Reuters](https://graphics.reuters.com/HEALTH-CORONAVIRUS/USA/qmypmkmwpra/) and MIT. Wealthier counties had less mobility than poorer ones.*

---

## Available data

To monitor mobility, there are several publicly available options:
* [Google's publicly available aggregated mobility data](https://www.google.com/covid19/mobility/)
* [Apple's publicly available aggregated mobility data](https://www.apple.com/covid19/mobility)

In Addition, through the World Bank's [Data Partnership](https://datapartnership.org/) the World Bank has access to several other datasets: 
* [Cuebiq](https://cuebiq.com)
* [Unacast](https://unacast.com)
* [Xmode](https://xmode.io)
* [Veraset](https://Veraset.com)
* [Facebook](https://dataforgood.fb.com/)


We will explore what we can do with each of the public datasets and some of the [Data Partnership](https://datapartnership.org/) ones. However, not every dataset represents every population equally. 

Some datasets may over-represent higher-income populations, while others may skew towards lower-income groups. 

## Exploring publicly available data

In early April 2020, the [World Bank Group designated several countries to receive COVID-19 support](https://www.worldbank.org/en/news/press-release/2020/04/02/world-bank-group-launches-first-operations-for-covid-19-coronavirus-emergency-health-support-strengthening-developing-country-responses). This list has since [expanded](https://www.worldbank.org/en/about/what-we-do/brief/world-bank-group-operational-response-covid-19-coronavirus-projects-list).  

We will focus on India, one of the countries receiving COVID-19 support.  However, this will work for any country where data is available. 


Let's start with exploring public mobility data from [Apple](https://www.apple.com/covid19/mobility) and [Google](https://www.google.com/covid19/mobility/). 


```python
# Import libraries, and designate where we keep our data folder.
import os
from pathlib import Path
from glob import glob
import pandas as pd

folder=Path("data/mobility")
```

### Apple mobility data


```python
# Read our downloaded Apple Data
apple_data=[p for p in folder.rglob('apple*.csv')]
apple_mov=pd.read_csv(apple_data[-1])

# Filter to include India
apple_mov=apple_mov[apple_mov['region']=='India']

# Pivot the data so that every date is a row.
apple_mov=apple_mov[apple_mov.columns[3:]].T
rename={56:'A-driving',
        57:'A-walking'}
apple_mov.rename(columns=rename, inplace=True)
apple_mov=apple_mov.iloc[1:]
apple_mov.head()
```




<div>
  <style scoped>
      .dataframe tbody tr th:only-of-type {
          vertical-align: middle;
      }

      .dataframe tbody tr th {
          vertical-align: top;
      }

      .dataframe thead th {
          text-align: right;
      }
  </style>
  <table border="1" class="dataframe">
    <thead>
      <tr style="text-align: right;">
        <th></th>
        <th>A-driving</th>
        <th>A-walking</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th>2020-01-13</th>
        <td>100</td>
        <td>100</td>
      </tr>
      <tr>
        <th>2020-01-14</th>
        <td>102.35</td>
        <td>99.18</td>
      </tr>
      <tr>
        <th>2020-01-15</th>
        <td>107.96</td>
        <td>104.08</td>
      </tr>
      <tr>
        <th>2020-01-16</th>
        <td>110.77</td>
        <td>107.41</td>
      </tr>
      <tr>
        <th>2020-01-17</th>
        <td>120.64</td>
        <td>113.91</td>
      </tr>
    </tbody>
  </table>
</div>


  <p/>


```python
# Set the date as the index, and create a simple plot
apple_mov.index = pd.to_datetime(apple_mov.index,format='%Y-%m-%d')
apple_mov.plot(marker='.', linestyle=':',figsize=(10, 5));
```


![png](/updates/mobility-metrics-in/output_10_0.png)


Apple's data is activity based, recording when people were walking and when they were driving. 

We can clearly see a sharp decline in walking / data activity recorded by Apple devices at the end of March. People with a higher income tend to own Apple devices.  This decline is consistent with our hypothesis.

Lastly, we see that in early May, people are starting to drive and walk more.

### Google mobility data


```python
# Read Google mobility data
google_data=[p for p in folder.rglob('Global_*.csv')]
google_mov=pd.read_csv(google_data[-1],low_memory=False)

# Filter down to the region
google_mov=google_mov[google_mov['country_region']=='India'] #Only India
google_mov=google_mov[google_mov['sub_region_1'].isnull()]  #Only national series
google_mov=google_mov[google_mov.columns[4:]]
# Rename the columns
rename={'retail_and_recreation_percent_change_from_baseline':'G-retail',
       'grocery_and_pharmacy_percent_change_from_baseline':'G-grocery',
       'parks_percent_change_from_baseline':'G-parks',
       'transit_stations_percent_change_from_baseline':'G-transit',
       'workplaces_percent_change_from_baseline':'G-workplaces',
       'residential_percent_change_from_baseline':'G-residential'}
google_mov.rename(columns=rename, inplace=True)

# Convert date strings into dates, and set the date as index.
google_mov['date']=google_mov['date'].apply(pd.Timestamp)
google_mov = google_mov.set_index('date')
google_mov.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>G-retail</th>
      <th>G-grocery</th>
      <th>G-parks</th>
      <th>G-transit</th>
      <th>G-workplaces</th>
      <th>G-residential</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-02-15</th>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>5.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-02-16</th>
      <td>2.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-02-17</th>
      <td>-1.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-02-18</th>
      <td>0.0</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-02-19</th>
      <td>0.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Show a simple plot
google_mov.plot(marker='.', linestyle=':',figsize=(10, 5));
```


![png](/updates/mobility-metrics-in/output_14_0.png)


Google's data is less about activity and more about location.  How much time did people spend in a given location? 

Note the changes after the end of March: 
* An increase in time spent in residential locations (people staying at home)
* An overall decrease in Retail, Grocery, Parks, Transit and Workplaces.

However, once the initial drop happened, we still observe:
* Spikes in Workplaces.  People across a wider diversity of incomes own Android phones.  Based on this chart, there are still a good number of people going back to their workplaces. 
* A drop in trips to retail locations, with a slight incrase in trips to grocery locations.  From this, we can infer that people are decreasing retail spending and saving the money for groceries.

### Comparing Apple and Google Data


```python
# Create a plot comparing Google and Apple data
ax = (apple_mov-100).plot(figsize=(10, 5))
(google_mov).plot(marker='.',linestyle=':',ax=ax);
```


![png](/updates/mobility-metrics-in/output_17_0.png)


Compairing Apple's activity data with Google location data, we make a few more observations:
* An increase in people staying in residential locations is [correctly] correlated with people walking and driving less.  
* The slightly increasing driving and walking rates towards the beginning of may could potentially be related to a slight uptick of trips to the grocery store.  

## Adding in Facebook mobility data

We are going to start with Facebook's new [Movement Range](https://devdatapartnership.herokuapp.com/Facebook.html#Movement-Range-maps) data.  This is not available to the general public, but it is available through the [Data Partnership](https://datapartnership.org/).  Submit a project proposal to use it!

Since this is still an early release, we get the data on `.csv`s


```python
fb_data=folder/'FB-mobility'
csvs=[p for p in fb_data.rglob('CSV*/in_gadm*.csv')]
csvs[-1]
```




    PosixPath('data/mobility/FB-mobility/CSVs20200413/in_gadm_mobility_statistics.20200413.csv')



We convert the data into a Pandas DataFrame.


```python
rename={'all_day_bing_tiles_visited_relative_change':'r_tiles',
        'all_day_ratio_single_tile_users':'r_tile'}

mov=pd.read_csv(csvs[-1])
mov.rename(columns=rename, inplace=True)
mov['ds']=mov['ds'].apply(pd.Timestamp)
mov = mov.set_index('ds')
mov.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>polygon_id</th>
      <th>area_type</th>
      <th>area_code</th>
      <th>polygon_name</th>
      <th>country_code</th>
      <th>r_tiles</th>
      <th>r_tile</th>
    </tr>
    <tr>
      <th>ds</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-04-08</th>
      <td>1132</td>
      <td>adm1</td>
      <td>IN.TG</td>
      <td>Telangana</td>
      <td>IN</td>
      <td>-0.574119</td>
      <td>0.141085</td>
    </tr>
    <tr>
      <th>2020-04-08</th>
      <td>1113</td>
      <td>adm1</td>
      <td>IN.JK</td>
      <td>Jammu and Kashmir</td>
      <td>IN</td>
      <td>-0.427373</td>
      <td>0.069458</td>
    </tr>
    <tr>
      <th>2020-04-12</th>
      <td>1132</td>
      <td>adm1</td>
      <td>IN.TG</td>
      <td>Telangana</td>
      <td>IN</td>
      <td>-0.557866</td>
      <td>0.155335</td>
    </tr>
    <tr>
      <th>2020-04-12</th>
      <td>1110</td>
      <td>adm1</td>
      <td>IN.GJ</td>
      <td>Gujarat</td>
      <td>IN</td>
      <td>-0.584159</td>
      <td>0.107713</td>
    </tr>
    <tr>
      <th>2020-04-12</th>
      <td>1102</td>
      <td>adm1</td>
      <td>IN.AR</td>
      <td>Arunachal Pradesh</td>
      <td>IN</td>
      <td>-0.463551</td>
      <td>0.055993</td>
    </tr>
  </tbody>
</table>
</div>



Facebook Movement Range data contains two types of data:

* **Mobility Range**: The degree of movement. How many map tiles were touched by users, relative to a baseline in the last months.

* **Stationary Share**: The percentage of users always on the same map tile, relative to a baseline last months.


Let's explore first the relation between them on a scatter plot:


```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

# Group by state
groups = mov.groupby('polygon_name')

# Plot
fig, ax = plt.subplots(figsize=(20, 10))
ax.margins(0.05) # Optional, just adds 5% padding to the autoscaling
for name, group in groups:
    ax.plot(group['r_tiles'], group['r_tile'], marker='.', linestyle='', label=name)
    ax.set_xlabel('Relative number of tiles')
    ax.set_ylabel('Relative number of single tile')
    ax.set_title('Relative Mobility')
ax.legend()

plt.show()
```


![png](/updates/mobility-metrics-in/output_27_0.png)


It seems clear, and logical, that there is a proportional relation: The more stationary a region is, the less movement range we see. A higher cluster around `0` (no relative change), means that in these regions, there is more variability of stationary populations.

We also see that some regions tend to be in the same space of the plot, especially the most compliant with the lockdown.

---

### Mobility range

This data shows the average number of level 16 Bing tiles (0.6km by 0.6km) that a Facebook user (mobile app + location history) was present in during a 24 hour period compared to pre-crisis levels. This is called the Travel Range map.


```python
import matplotlib.dates as mdates

ax=mov.groupby('polygon_name')['r_tiles'].plot( marker='.', linestyle=':',figsize=(20, 15));
plt.legend();
```


![png](/updates/mobility-metrics-in/output_30_0.png)


ALL regions see a significant decrease in range. The degree of change seems mostly constant, and some places twice than others.

---

### Stationary share

Percentage of Facebook users (mobile app + location history) that were present in only one such level 16 Bing tile in at least 3 different hours of the day. 


```python
# Trick to order by last value
mov.sort_values(by=['r_tile'],ascending=False, inplace=True)
```


```python
print("The range of value is from %.3f to %.3f"%(mov['r_tile'].min(),mov['r_tile'].max()))
```

    The range of value is from 0.020 to 0.284



```python
ax=mov.groupby('polygon_name', sort=False)['r_tile'].plot( marker='.', linestyle=':',figsize=(20, 15));
plt.legend()
```




    <matplotlib.legend.Legend at 0x1185cbac8>




![png](/updates/mobility-metrics-in/output_35_1.png)


We see that some regions see little change, while others see `~x15` increase. The capital "NCT of Delhi" is the region with the highest increase of stationay users, while Himachal Pradesh, a remote mountainous region varely saw any change.

---

### Break up by Indian State


```python
import matplotlib.dates as mdates

groups = mov.groupby('polygon_name')

# Plot
fig, ax = plt.subplots(figsize=(20, 40),nrows=12, ncols=3,sharex=False, sharey=True)
i=0
for name, group in groups:
    row=int(np.floor(i/(ax.shape[1])))
    col=i%(ax.shape[1])
    i=i+1
    #print(i,row,col)
    ax[row,col].plot(group['r_tiles'], marker='.', linestyle='', label=name)
    ax[row,col].margins(0.05)
    ax[0,0].set_xlabel('Relative number of tiles')
    ax[row,0].set_ylabel('Relative number of single tile')
    ax[row,col].xaxis.set_major_locator(mdates.WeekdayLocator())
    ax[row,col].xaxis.set_major_formatter(mdates.DateFormatter('%b %d'))
    ax[row,col].grid(True)
    ax[row,col].legend()

plt.subplots_adjust(left = 0.125,right = 0.9,bottom = 0.1,top = 0.9,wspace = 0,hspace = 0.2)
plt.show()
```


![png](/updates/mobility-metrics-in/output_38_0.png)


## Comparing Apple, Google and Facebook data


```python
ax = (apple_mov-100).plot(figsize=(15, 5))
(google_mov).plot(marker='',linestyle=':',ax=ax)
(mov*100).groupby('polygon_name')['r_tile'].plot(ax=ax,marker='*', linestyle='',color='black',alpha=0.2);
(mov*100).groupby('polygon_name')['r_tiles'].plot(ax=ax,marker='+', linestyle='',color='black',alpha=0.2);
```


![png](/updates/mobility-metrics-in/output_40_0.png)


Apple mobility data (solid colored lines) tend to show among the strongest reduction of mobility, consistent with wealthier customers that can afford Apple products and can also work from home or restrict their mobility. 

Google mobility data (dotted colored lines) breaks down mobility. We can see strongest reduction of mobility in retail and transit, comparable to Apple data. Groceries and workplaces remain with 40% mobility versus the baseline.

Facebook data (black crosses and stars) is broken down into states, and shows a significant range. This is expected as some states are largely rural and some urban, with different livelihoods and mobility patterns.

## Review

In this post, you have learned how to work with [Apple](https://www.apple.com/covid19/mobility), [Google](https://www.google.com/covid19/mobility/), and [Facebook Movement Range](https://devdatapartnership.herokuapp.com/Facebook.html#Movement-Range-maps) data.

We compared the three data sources, and discussed potential bias and its implications in mobility data.  

If you are a Development Partner, and would like to use Data Partnership data, submit a project!

If you are a Data Provider and want to see your data be used for world-changing impact, [sign up to work](mailto:datapartnership@worldbank.org) with the Data Partnership!

---
_Authors:_ 
* [_Maksim Pecherskiy_](https://maksimpecherskiy.com)  _Lead Data Engineer, Development Data Partnership_
* [_Bruno Sanchez-Andrade Nuno_](https://brunosan.eu/)  _Lead Data Science Advisor, Development Data Partnership_
