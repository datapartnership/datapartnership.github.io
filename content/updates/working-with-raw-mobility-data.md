+++
date = 2020-05-21T20:12:14Z
title = "Working with raw mobility data"
type = "Article"
draft = true
slug = "working-with-raw-mobility-data"
+++

In the previous post, we wrote about how to use data from Apple, Google and Facebook to understand mobility during social distancing.

But what if you have device-level data (e.g. data from `Cuebiq`), but want to aggregate your own metrics, similar to the ones we explored in [Mobility during social distancing](/updates/understanding-mobility-during-social-distancing)?

To do this, we will use real locations of `markets` in Kolkata, India from OSM.  We will simulate device-level data using random points at random times.  

### Getting markets' locations
We start off by getting the market location data from OSM using `amenity=marketplace`. We use [overpass-turbo](https://overpass-turbo.eu/s/TO4) We read the file into `geopandas` and get the centroids of those markets.


```python
import pyproj
pyproj.Proj("+init=epsg:4326")
```




    pyproj.Proj('+units=m +init=epsg:4326', preserve_units=True)




```python
# Get locations of real amenity=marketplace from OSM
import geopandas as gpd
import contextily as ctx
marketplace_file=folder/'Kolkata-marketplace.geojson'

# Read the geojson file and convert it to EGPS3857
markets = gpd.read_file(marketplace_file).to_crs({'init': 'epsg:3857'})

# Convert the geometry to centroids as opposed to polygons. 
markets['geometry'] = markets['geometry'].centroid

# Create a simple plot of the markets
ax = markets.plot(figsize=(10, 10), alpha=0.8,color='red')
ctx.add_basemap(ax)
ax.set_axis_off()
print("There are %i markets in the area"%len(markets))
```

    There are 31 markets in the area



![png](/updates/mobility-metrics-in/output_45_1.png)


### Creating a market buffer
We add a buffer of 500m around them so we assign any device-level ping as a visit to the market.


```python
# Add a 500m buffer
markets_zones=markets
markets_zones.geometry=markets.geometry.buffer(500)

# Make a simple plot
ax = markets.plot(figsize=(10, 10), alpha=0.8,color='red')
ctx.add_basemap(ax)
```


![png](/updates/mobility-metrics-in/output_47_0.png)


### Creating simulated dates
In order to create simulated data, we need some code to generated simulated dates.  Here we have some code that will generate a random date between a provided interval.


```python
import random
import time

def strTimeProp(start, end, format, prop):
    """Get a time at a proportion of a range of two formatted times.

    start and end should be strings specifying times formated in the
    given format (strftime-style), giving an interval [start, end].
    prop specifies how a proportion of the interval to be taken after
    start.  The returned time will be in the specified format.
    """

    stime = time.mktime(time.strptime(start, format))
    etime = time.mktime(time.strptime(end, format))

    ptime = stime + prop * (etime - stime)

    return time.strftime(format, time.localtime(ptime))


def randomDate(start, end, prop):
    return strTimeProp(start, end, '%d/%m/%Y %I:%M %p', prop)

```

### Create random pings at random times
Now we are ready to generate synthetic data.  We will generate random pings at random times.  These pings will be limited by a bounding box of all the markets, but each ping does necessarily have to fall into a market.


```python
# Create synthtetic data

import numpy as np
from shapely.geometry import Point

xmin, ymin, xmax,  ymax = markets.total_bounds
num_points = 200000
xc = (xmax - xmin) * np.random.random(num_points) + xmin
yc = (ymax - ymin) * np.random.random(num_points) + ymin
tc = [randomDate("4/1/2020 1:01 AM", "20/4/2020 04:50 AM", random.random()) for i in np.empty(num_points)]
points_geom = gpd.GeoSeries([Point(x, y) for x, y in zip(xc, yc)])
points = gpd.GeoDataFrame(pd.DataFrame({'time':tc}),geometry=points_geom, crs=markets.crs)
points.crs = markets.crs
points.head()
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
      <th>time</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>28/02/2020 07:45 PM</td>
      <td>POINT (9845269.370 2585592.069)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21/02/2020 07:41 PM</td>
      <td>POINT (9838860.837 2568362.007)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>09/02/2020 12:34 PM</td>
      <td>POINT (9835194.903 2584734.155)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16/01/2020 01:25 AM</td>
      <td>POINT (9834310.364 2586797.338)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>06/03/2020 03:27 AM</td>
      <td>POINT (9833397.813 2583220.593)</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a simple plot of the markets.
ax =  markets.plot(figsize=(10, 10), alpha=.7,color='red')
points.sample(n=int(num_points/100)).plot(ax=ax, alpha=0.1,color='blue')

ctx.add_basemap(ax)
ax.set_axis_off()
```


![png](/updates/mobility-metrics-in/output_52_0.png)


### Determine which pings are within a market boundary.
With a Spatial join, we can filter out any point not intersecting our buffered marketplaces.


```python
inside=gpd.sjoin(points, markets, how='left',)
inside=inside[inside.notnull()['id']]

# Create a plot of the points that are inside markets.
ax =  markets.plot(figsize=(10, 10), alpha=.7,color='red')
inside.plot(ax=ax, alpha=0.01,color='blue')

ctx.add_basemap(ax)
ax.set_axis_off()
```


![png](/updates/mobility-metrics-in/output_55_0.png)


### Date-Binning of Supermarket Mobility
Now, we need to bin the market mobility by date:


```python
# Make the date string into a datetime 
inside['time'] = pd.to_datetime(inside['time'], format='%d/%m/%Y %I:%M %p')

# Create a day column
inside['day'] = pd.to_datetime(inside['time'], format='%d/%m/%Y').dt.strftime('%m/%d/%Y')

# Create a simple plot showing market mobility in India!
ax = inside.groupby('day').count()['time'].plot()
ax.set_title('Supermarket Mobility');
```


![png](/updates/mobility-metrics-in/output_58_0.png)


If you have device level data, such as that provided by the Data Partners in the [Data Partnership](https://datapartnership.org/), you can do this for any type of location in any place of the world.  

Being able to measure mobility this way is a powerful tool for analyzing the impacts of social distancing, and adjusting policies to help prevent the spread of COVID-19.