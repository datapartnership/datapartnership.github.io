+++
title = "From Data to Cleaner Air: How Data Partnerships Are Helping Cities Tackle Air Pollution"
authors = ["Development Data Partnership Team"]
categories = ["Case Study"]
partner = ["GrabMaps", "Quadrant", "VesselBot", "Veraset", "Waze"]
dev_partner = ["Asian Development Bank", "World Bank"]
tags = ["Transport"]
date = 2026-09-04T00:00:00Z
+++

Improving air quality is essential to sustainable development because air pollution harms public health, reduces economic productivity, and diminishes people’s quality of life.

Every year on September 7, the International Day of Clean Air for Blue Skies calls for action to enhance air quality. The theme of 2026 focuses on the economic case for tackling air pollution and climate change together. For cities, turning that case into effective policy requires evidence backed by data: where emissions come from, who is exposed, and how different interventions could change traffic and pollution.

These questions are particularly difficult in places where conventional travel surveys and monitoring systems are limited, costly, or difficult to update. Through the Development Data Partnership, technology companies are providing mobility and transport data that development organizations can combine with public data and policy models. The following three projects showcase how these collaborations are helping examine and tackle traffic-related air pollution.


<figure style="text-align: center;">
  <img src="from-data-to-cleaner-air-how-data-partnerships-are-helping-cities-tackle-air-pollution_thumbnail.png" alt="clean air day thumbnail" style="max-width: 100%;">
</figure>


## Testing transport policies in Southeast Asian cities

Cities across Southeast Asia are considering measures such as low-emission zones and vehicle restrictions, but these interventions can affect areas beyond their intended purposes. Restricting vehicles in one area may reduce emissions there but also divert traffic to nearby roads. To help cities compare options without conducting a separate transport study for every policy design, the Asian Development Bank developed a simulation pipeline that combines transport and emissions models. It uses GPS and traffic data, road networks, vehicle-fleet information, and locally calibrated emission factors to estimate traffic conditions and pollutants. Anonymized mobile location data from [Quadrant](https://www.quadrant.io) helps identify travel patterns and construct origin-destination matrices; [GrabMaps](https://grabmaps.grab.com) data helps calibrate and validate modeled traffic conditions against observed conditions; and [VesselBot](https://www.vesselbot.com)'s vessel-emissions data allows the team to combine port-related emissions with road-traffic emissions. The model can test restrictions based on vehicle type, time of day, or emissions standard and estimate their effects inside and outside low-emission zones, including on major road segments and at different times of day.

For instance, in Bangkok, the analysis estimated that activity associated with Bangkok Port accounted for 1.2 percent of citywide carbon monoxide emissions from transport, 9.1 percent of nitrogen oxides, and 2.1 percent of PM10. This suggested that the port made a relatively small contribution to particulate matter from transport but a larger contribution to nitrogen oxides. Read more [here](https://datapartnership.org/updates/helping-southeast-asian-cities-improve-air-quality-with-data).


## Comparing transport pathways across thousands of cities

The World Bank's Passenger Transport Emissions Pathways (PATH) model provides comparable estimates of passenger transport demand, greenhouse gas emissions, and local air pollutants across nearly 4,000 urban areas in 116 low- and middle-income countries. It estimates total passenger travel distances, distributes that total among private cars, two- and three-wheelers, public transport, and nonmotorized transport—accounting for how costs, infrastructure, and urban layout influence people's choices—and applies mode- and fuel-specific emission factors to translate transport activity into emissions. Aggregated and anonymized mobility data from [Veraset](https://www.veraset.com), provided through the Development Data Partnership, forms an important part of this approach. 

After applying quality checks to location signals with coordinates and timestamps, the team aggregates the data across devices to estimate average daily travel distances and wider city-level mobility patterns. The analysis focuses on aggregate trends rather than tracking individuals and can produce consistent estimates where conventional travel surveys may be unavailable.

Under PATH's business-as-usual scenario, urban passenger transport emissions are projected to more than double by 2050. The model also shows why the same policy will not produce the same results everywhere. Public transport investment tends to have its greatest effect in megacities, while electric-vehicle adoption may have a larger effect in other highly motorized urban areas. Policymakers can use the model to test how changes in prices, infrastructure, land use, and technology could affect travel behavior and emissions in a particular urban context to improve air quality. Find out more [here](https://datapartnership.org/updates/a-data-driven-model-for-urban-transport-emissions-leveraging-global-mobility-data-in-low-and-middle-income-countries).


## Linking congestion and air pollution in Tbilisi

In Tbilisi, Georgia, a World Bank study supported by Sweden examined how exposure to air pollution varied across income groups and occupations, and how traffic contributed to the problem. Through the Development Data Partnership, the research team used [Waze for Cities](https://www.waze.com/wazeforcities) data covering 2019 to 2021. The dataset provided speed and waiting-time information for road segments at five-minute intervals. The team classified speeds below 5 kilometers per hour as severe congestion, mapped the affected road segments, aggregated the information by census block and day, and examined the relationship between traffic-jam days and PM2.5 levels.

The analysis found that traffic jams were concentrated closer to the city center and became less frequent farther away. It also found a positive correlation between traffic-jam days and air pollution: as the number of traffic-jam days increased, the rise in air pollution was consistent and proportional. Because the study describes this result as a correlation, it should not be read as proof that traffic alone caused the change. The findings can help inform measures discussed by the project team, including carbon pricing to reduce the number of vehicles on the road, incentives for public transport, and improvements to urban planning. Discover more [here](https://datapartnership.org/updates/role-of-traffic-related-emissions-on-air-pollution-in-tbilisi).


## Turning private-sector data toward the public good

Data plays a key role in understanding the sources, scale, and distribution of air pollution and in designing more effective measures to address it.

In each of the three projects, data partners fill a gap that conventional sources alone may not cover. GrabMaps, Quadrant, VesselBot, Veraset, and Waze provide valuable insights into traffic, mobility, and emissions. Combined with in-depth analysis, these private data sources can help policymakers turn broad commitments to cleaner air into evidence-informed decisions.









