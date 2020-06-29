+++
date = 2020-06-24T04:00:00Z
partner = ""
title = "How to Find Population Movement Data for Your Country: The Mobile Location Data Inventory"
type = "Article"
url = "find-population-movement-data-for-your-country"

+++
As the COVID-19 pandemic continues, so have measures to mitigate its spread. Physical distancing and [movement restrictions](https://ourworldindata.org/policy-responses-covid#restrictions-on-internal-movement) – such as school and workplace closures, public event cancellations, stay-at-home orders, and international and domestic travel bans – vary considerably [across countries](https://openknowledge.worldbank.org/bitstream/handle/10986/33754/Determinants-of-Social-Distancing-and-Economic-Activity-during-COVID-19-A-Global-View.pdf?sequence=1&isAllowed=y).


Since movement restrictions themselves can have profoundly negative impacts on communities, it is essential for governments to understand which policies have the most and least effective impacts.

Anonymized “mobility data” – GPS data generated primarily from smartphones – may support efforts to limit movement restriction measures to those most needed, by painting a better picture of population frequency of movement, distance of movement relative to home, daily commuting patterns, inter-district flows, and social mixing.

A number of companies have generously made these data available, either publicly or through the [Development Data Partnership](https://datapartnership.org/), to support COVID-19 response programs.

With so many new sources of information, it can be difficult to know where to start. To overcome this challenge, the Partnership, in collaboration with the World Bank's Sustainable Development Data Lab and Health and Nutrition Program, created a Mobile Location Data Inventory, which shows the availability of 17 mobility-related datasets in 90 developing economies.

Click below to explore the data!

<a href="/mobility-data-dashboard-main" target="_blank"><img src="/updates/mobility-dashboard/mobility-dashboard-optimized.gif" style="width:100%;"/></a>

_Visualization by Hellen Matarazzo, Data Scientist, Health Global Practice, The World Bank_

#### **Apple and Google Mobility Reports**

Google launched the [Community COVID-19 Mobility Reports](https://www.google.com/covid19/mobility/) for 131 countries and regions following the outbreak. Apple released [Mobility Trends Reports](https://www.apple.com/covid19/mobility) for 63 countries, regions, and cities. Both the datasets compare the change in mobility patterns to a baseline period. While Google insights focus on movements to and from specific business types (e.g., pharmacies, retail, parks, residential), Apple insights, which are based on Apple Maps direction requests, focus on movement by mode of transportation, such as pedestrian movement and public transport.

#### **Facebook Movement Data**

The [Facebook Data for Good program](https://dataforgood.fb.com/) has released four new datasets pertaining to mobility:

• [**Movement Range**](https://dataforgood.fb.com/tools/movement-range-maps/) comprises two metrics: [Change in Movement and Stay Put](https://research.fb.com/blog/2020/06/protecting-privacy-in-facebook-mobility-data-during-the-covid-19-response/). The Change in Movement metric is an estimate of the number of the people who are “moving”, i.e., the average number of level 16 [Bing tiles](https://docs.microsoft.com/en-us/bingmaps/articles/bing-maps-tile-system) (0.6km by 0.6km) that a user was present in during an entire day versus a pre-pandemic baseline. The Stay Put metric estimates the opposite – the number of people who are staying home, i.e., percent of users that were present in only one level 16 Bing tile for at least 3 different hours on a given day. The [COVID-19 Mobility Data Network](https://www.covid19mobility.org/) provides a visualization of these [publicly available maps](https://data.humdata.org/dataset/movement-range-maps) for 14 countries.

• **Colocation Maps** present the probability of a co-location event between users from different admin level 3 regions, predicting the rate at which people in particular towns and cities where COVID-19 cases exist and interconnect with groups of people in other places.

• **Travel Patterns** examine international travel patterns of Facebook users (who have opted into sharing location data on their mobile app), comparing users’ movement across long distances between countries, especially via air and train travel. The counts of travel patterns are updated daily. These maps can be used to understand the economic impact and recovery of the travel and tourism industries.

• **Social Connectedness Index** looks at the relative probability of friendship ties between admin level 3 or 2 or 1 region, globally. This metric may be used as a covariate layer for [predictive models for the spread of the disease](https://www.theguardian.com/world/2020/apr/14/facebook-friendships-can-help-predict-covid-19-spread-study-finds).

#### **Location Data and Mobility Insights**

The Development Data Partnership has an agreement with four location intelligence companies – [Unacast](https://www.unacast.com/), [Cuebiq](https://www.cuebiq.com/),[Veraset](https://www.veraset.com/) and [X-Mode](https://www.xmode.io/). They provide near real-time mobility data, which can be used to target and design measures in response to COVID-19. The unit of measurement is a single GPS point. Utilizing such datasets, [CARTO](https://carto.com/) provides location data analysis and visualization through their platform and spatial data repository.

Mapbox offers [traffic](https://www.mapbox.com/traffic-data/?utm_medium=blog&utm_source=mapbox-blog&utm_campaign=blog%7Cmapbox-blog%7Ctelemetry%7Ctravel-changes-around-the-world-from-covid-19-cc79db7e04c7-20-03&utm_term=telemetry&utm_content=travel-changes-around-the-world-from-covid-19-cc79db7e04c7) and [telemetry density data](https://www.mapbox.com/telemetry/) for mobility analysis. As telemetry data is collected only from moving devices, changes in movement can be observed at a larger scale, especially for map improvisation and traffic prediction.

You can use the visualization below to pick which dataset suits your needs based on the country you want to analyze. We are working hard to structure the data across providers in a similar way, so that you can use the same code to analyze data from all providers.

<a href="/mobility-data-dashboard-bycountry" target="_blank"><img src="/updates/mobility-dashboard/mobility-dashboard-bycountry-optimized.gif" style="width:100%;"/></a>

#### **And...**

Don't see your data here and want to showcase the availability of your mobility data? Interested in learning how to work with these data? [Write to us](datapartnership@worldbank.org)!

_Authors:_

**Rajee Kanagavel**, SD Data Lab Coordinator, _World Bank Sustainable Development Data Lab_

**Maksim Pecherskiy**, Lead Data Engineer, _Development Data Partnership_

_World Bank Sustainable Development Data Lab is a community of practice for the Bank staff seeking to modernize project data practices through learning, networking, and shared resources._

_The Development Data Partnership is a partnership between international organizations and companies, created to facilitate the use of third-party data in research and international development._
