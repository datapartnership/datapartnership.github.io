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

Private sector data can be a great asset in helping governments measure how their policies impact human movement within different communities.

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

## Private sector publicly available data

To monitor mobility, there are several publicly available options from the private sector:
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


### Apple mobility data



![png](/updates/mobility-metrics-in/output_10_0.png)


Apple's data is activity based, recording when people were walking and when they were driving. 

We can clearly see a sharp decline in walking / data activity recorded by Apple devices at the end of March. People with a higher income tend to own Apple devices.  This decline is consistent with our hypothesis.

Lastly, we see that in early May, people are starting to drive and walk more.

### Google mobility data


![png](/updates/mobility-metrics-in/output_14_0.png)


Google's data is less about activity and more about location.  How much time did people spend in a given location? 

Note the changes after the end of March: 
* An increase in time spent in residential locations (people staying at home)
* An overall decrease in Retail, Grocery, Parks, Transit and Workplaces.

However, once the initial drop happened, we still observe:
* Spikes in Workplaces.  People across a wider diversity of incomes own Android phones.  Based on this chart, there are still a good number of people going back to their workplaces. 
* A drop in trips to retail locations, with a slight incrase in trips to grocery locations.  From this, we can infer that people are decreasing retail spending and saving the money for groceries.

### Comparing Apple and Google Data

![png](/updates/mobility-metrics-in/output_17_0.png)


Comparing Apple's activity data with Google location data, we make a few more observations:
* An increase in people staying in residential locations is [correctly] correlated with people walking and driving less.  
* The slightly increasing driving and walking rates towards the beginning of may could potentially be related to a slight uptick of trips to the grocery store.  

## Adding in Facebook mobility data

We are going to start with Facebook's new [Movement Range](https://devdatapartnership.herokuapp.com/Facebook.html#Movement-Range-maps) data.  This is not available to the general public, but it is available through the [Data Partnership](https://datapartnership.org/).  Submit a project proposal to use it!


Facebook Movement Range data contains two types of data:

* **Mobility Range**: The degree of movement. How many map tiles were touched by users, relative to a baseline in the last months.

* **Stationary Share**: The percentage of users always on the same map tile, relative to a baseline last months.


Let's explore first the relation between them on a scatter plot:


![png](/updates/mobility-metrics-in/output_27_0.png)


It seems clear, and logical, that there is a proportional relation: The more stationary a region is, the less movement range we see. A higher cluster around `0` (no relative change), means that in these regions, there is more variability of stationary populations.

We also see that some regions tend to be in the same space of the plot, especially the most compliant with the lockdown.

---

### Mobility range

This data shows the average number of level 16 Bing tiles (0.6km by 0.6km) that a Facebook user (mobile app + location history) was present in during a 24 hour period compared to pre-crisis levels. This is called the Travel Range map.

![png](/updates/mobility-metrics-in/output_30_0.png)


ALL regions see a significant decrease in range. The degree of change seems mostly constant, and some places twice than others.

---

### Stationary share

Percentage of Facebook users (mobile app + location history) that were present in only one such level 16 Bing tile in at least 3 different hours of the day.


![png](/updates/mobility-metrics-in/output_35_1.png)


We see that some regions see little change, while others see `~x15` increase. The capital "NCT of Delhi" is the region with the highest increase of stationay users, while Himachal Pradesh, a remote mountainous region varely saw any change.

---

### Break up by Indian State


![png](/updates/mobility-metrics-in/output_38_0.png)


## Comparing Apple, Google and Facebook data

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
