+++
date = 2020-03-19T20:12:14Z
partner = "Facebook and Mapbox"
title = "Addressing COVID-19 through Public-Private Data Partnerships -- Where Do We Put New Testing Facilities?"
type = "Case Study"

+++
**Background**

As the COVID-19 pandemic evolves, the World Bank is ramping up its response to help countries around the world. The Bank recently announced [$14 billion emergency support program](https://www.worldbank.org/en/news/press-release/2020/03/17/world-bank-group-increases-covid-19-response-to-14-billion-to-help-sustain-economies-protect-jobs), for which a key pillar is to help countries strengthen their capacity to monitor and respond to the spread of the virus. Countries like China, South Korea, and Singapore have effectively leveraged big data to do this -- with solutions like apps that track the transmission chains of confirmed cases or proactive alerts for people to get tested at a nearby facility. Big data has proven to be a critical tool for monitoring and responding to outbreaks.

The problem is, not all countries can easily benefit from big data, whether for lack of adequate infrastructure, technical capacity, financial resources, or needed regulatory policy. This is one of the reasons the World Bank, along with the IMF and the Inter-American Development Bank, founded the [Development Data Partnership](https://datapartnership.org). The Partnership removes barriers to collaboration between the private sector and international organizations, so that members may pool private and open data to create “data goods” – insights and code that can be used by the public sector for good. During the COVID-19 pandemic, Development Data Partnership team members are working tirelessly to leverage big data to support our colleagues’ prevention, response, and recovery efforts. In this blog, we’ll look at a proof of concept for where big data could be used to predict where extra test facilities and hospital beds may be most needed.

**Where Do We Need Extra Test Facilities?**

This was one of the key questions posed by experts at a recent COVID-19 response meeting. How can we offer approximate answers to this question when we don’t have good - or any - data? We need to know where the vulnerable people are and where they could go to receive medical support. What we present here is a proof of concept that demonstrates the value of partnering with the private sector and public sector, while also leveraging open data. We worked with Facebook’s [Disease Prevention Maps](https://dataforgood.fb.com/tools/disease-prevention-maps/) – a global map with population density estimates, sufficient for our purposes. We then needed to determine location of health facilities – accessible via government partners, or, in a pinch, through open data (e.g., [Open Street Map](https://www.openstreetmap.org/#map=4/38.01/-95.84)).

The distance to health facilities should not be calculated in meters or miles on a straight line, but in minutes traveling on actual roads. This can be a challenge in developing countries, with a complex mix of national, local, and often unpaved roads leading to a health clinic. To solve this challenge, we worked with [Mapbox](https://www.mapbox.com/) and their global [routing Matrix engine](https://docs.mapbox.com/help/glossary/matrix-api/). We can then use their API to map, at a national scale, how far away, in minutes, anyone is from their closest health facility, when traveling by road. Conversely, we can determine what areas and populations are served by each hospital. For illustrative purposes, we chose Spain to run the model, but the system would be analogous for any country.

![Mapbox Spain](/Mapbox-Spain.png)

[_Click for an interactive map_](https://api.mapbox.com/styles/v1/brunosan/ck7tmxdao01og1io2d4wir16v.html?fresh=true&access_token=pk.eyJ1IjoiYnJ1bm9zYW4iLCJhIjoic3FUc1dJWSJ9.v0525WacYWcsHM1KtbZitg#4.89/40.89/-3.22) _Access to health facilities in Spain: Each flag represents a health facility. In green are locations within 1-hour driving distance to their closest health facility. Yellow places up to 1.5 hours and in red, the closest health facility is more than 2 hours driving. The opacity of the color is proportional to the population. Empty areas are transparent, while very populous areas are opaque._

We can also see what geographical areas are served by each hospital:

![Closest Hospital](/Closest-Hospital-Spain.png)

_Map of Spain color coded by the regions where each hospital is closest_

More importantly, we can also build geographic histograms to see what percentage of the population is furthest from a facility – for example, more than 1-hour driving distance (\~1.6% of the population for Spain) and where these people are.

![Spain Population](/Spain-Population.png)

_Right: Cumulative histogram of people’s distance to their closest health facility driving. Red line indicates 1 hour, which corresponds to the 1.61% of the population who are a further distance. Left: Map of Spain, with health facilities in blue. In red are the locations where their distance to the closest health facility is further than 1 hour of driving._

_Data Informed_

With these preliminary data at hand, we can begin a more efficient and _data informed_ conversation on additional information we would need to expand health testing capacity. We know how many people are covered and to what degree of proximity, so working with our health colleagues, we can start the discussion on other critical supply and demand factors. If no further data are available, we can use these rough analytics to help prioritize current locations serving the most amount of people? Do we locate them where the population live furthest? Is it better to have mobile clinics itinerant on the “red zones”?

**Where Do We Need Extra Beds?**

This is another key question that needs to be answered during COVID-19, as we need to know how prepared our existing health care facilities are to accommodate patients. While modelling the rate of infection is an extremely complex and data sensitive process, it is still nonetheless extremely important to estimate the degree to which facilities might need extra beds.

Developed countries might have this information updated regularly, but many developing countries still need tools to estimate the demand for their scarce resources and where to deploy support.

To help answer this question, we again use Facebook’s “Disease Prevention Maps” but this time instead of using the estimates for population in general, we will use their estimations of people over 60 years old. We know that the virus affects disproportionately older people. With lack of better disaggregated data, we use this number as a proxy of demand for beds. That is, the testing system will give us a rate of infection, and the elderly population will give us, proportionally to that, the demand on each hospital for beds. We also remove from the database health facilities without beds (clinics for doctor visits), and we cluster all hospitals within 5 kms of each other, since we assume that we can move patients between them (for example, in big cities). The rest of the calculation is essentially the same, and at the end we can plot a supply/demand scatter plot of beds (supply) and elderly population for which a hospital that has a given number of beds is the closest (demand):

![Demand and Supply](/Demand-Supply.png)

_Demand/Supply of hospital beds:_ _On the horizontal axis we have the (supply) number of beds for a given cluster of hospitals in close proximity, while on the vertical axis we have the (demand) number of elderly people for the cluster where a given number of beds is the closest._

In this crude model, we can add data as we have gathered it. For example, actual number of confirmed cases or actual supply of unused ICU beds/respirators. Furthermore, we can filter locations on the map where the demand is highest and the supply lowest (i.e., the top-left quadrant). The clusters of hospitals could be then be mapped out:

![Elderly Population Map](/Elderly-Population-Map.png)

_Locations where, according to this rough model, the demand by a significant elderly population is highest compared of supply of beds in their vicinity._

**Open Code**

The code to replicate and improve this model is [available here](https://github.com/datapartnership/covid19/blob/master/accessibility-Spain.ipynb)

_Author:_ [_Bruno Sanchez-Andrade Nuno_](https://brunosan.eu/)  _Lead Data Science Advisor, Development Data Partnership;_ [_Holly Krambeck_](https://www.linkedin.com/in/holly-krambeck)_, Senior Economist, World Bank and founder, Development Data Partnership_