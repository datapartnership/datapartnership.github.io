+++
date = 2019-07-01T20:12:14Z
draft = true
partner = "Facebook and Mapbox"
title = "Addressing COVID-19 through Public-Private Data Partnerships"
type = "Case Study"

+++
**Background**

As the COVID-19 pandemic evolves, the World Bank is ramping up its response to help countries around the world. The Bank recently announced [$14 billion emergency support program](https://www.worldbank.org/en/news/press-release/2020/03/17/world-bank-group-increases-covid-19-response-to-14-billion-to-help-sustain-economies-protect-jobs), for which a key pillar is to help countries strengthen their capacity to monitor and respond to the spread of the virus. Countries like China, South Korea, and Singapore have effectively leveraged big data to do this -- with solutions like apps that track the transmission chains of confirmed cases or proactive alerts for people to get tested at a nearby facility. Big data has proven to be a critical tool for monitoring and responding to outbreaks.

The problem is, not all countries can easily benefit from big data, whether for lack of adequate infrastructure, technical capacity, financial resources, or needed regulatory policy. This is one of the reasons the World Bank, along with the IMF and the Inter-American Development Bank, founded the [Development Data Partnership](https://datapartnership.org). The Partnership removes barriers to collaboration between the private sector and international organizations, so that members may pool private and open data to create “data goods” – insights and code that can be used by the public sector for good. During the COVID-19 pandemic, Development Data Partnership team members are working tirelessly to leverage big data to support our colleagues’ prevention, response, and recovery efforts. In this blog, we’ll look at a proof of concept for where big data could be used to predict where extra test facilities and hospital beds may be most needed.

**Where Do We Need Extra Test Facilities?**

This was one of the key questions posed by experts at a recent COVID-19 response meeting. How can we offer approximate answers to this question when we don’t have good - or any - data? We need to know where the vulnerable people are and where they could go to receive medical support. What we present here is a proof of concept that demonstrates the value of partnering with the private sector and public sector, while also leveraging open data. We worked with Facebook’s [Disease Prevention Maps](https://dataforgood.fb.com/tools/disease-prevention-maps/) – a global map with population density estimates, sufficient for our purposes. We then needed to determine location of health facilities – accessible via government partners, or, in a pinch, through open data (e.g., [Open Street Map](https://www.openstreetmap.org/#map=4/38.01/-95.84)).

The distance to health facilities should not be calculated in meters or miles on a straight line, but in minutes traveling on actual roads. This can be a challenge in developing countries, with a complex mix of national, local, and often unpaved roads leading to a health clinic. To solve this challenge, we worked with [Mapbox](https://www.mapbox.com/) and their global [routing Matrix engine](https://docs.mapbox.com/help/glossary/matrix-api/). We can then use their API to map, at a national scale, how far away, in minutes, anyone is from their closest health facility, when traveling by road. Conversely, we can determine what areas and populations are served by each hospital. For illustrative purposes, we chose Spain to run the model, but the system would be analogous for any country.

![](/Mapbox Spain.png)

[Click for an interactive map](https://api.mapbox.com/styles/v1/brunosan/ck7tmxdao01og1io2d4wir16v.html?fresh=true&access_token=pk.eyJ1IjoiYnJ1bm9zYW4iLCJhIjoic3FUc1dJWSJ9.v0525WacYWcsHM1KtbZitg#4.89/40.89/-3.22) Access to health facilities in Spain: Each flag represents a health facility. In green are locations within 1-hour driving distance to their closest health facility. Yellow places up to 1.5 hours and in red, the closest health facility is more than 2 hours driving. The opacity of the color is proportional to the population. Empty areas are transparent, while very populous areas are opaque.

**Solution**: Working with Where Is My Transport, map Freetown’s formal and informal transport network, and make the data freely available to governments, academia, and NGOs.

**Impact**: The local government can now identify and prioritize interventions that will enhance the resiliency of their transport system, to maximize economic and social benefits.

![Freetown Flooding](/uploads/WIMT_Flood.png "Freetown Flooding")