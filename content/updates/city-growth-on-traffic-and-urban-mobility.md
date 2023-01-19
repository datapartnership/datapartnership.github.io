+++
date = 2023-01-19T00:00:00Z
title = "The Effects of City Growth on Traffic and Urban Mobility"
authors = ["Cal Poly", "Gabriel Vicente"]
categories = ["Case Study"]
dev_partner = ["World Bank"]
partner = ["Waze"]
tags = ["Disaster Risk Management", "Transport"]
links = [
    "https://dxhub.calpoly.edu/challenges/city-growth-on-traffic-and-urban-mobility","https://github.com/datapartnership/effects-of-city-growth-patterns-on-traffic-and-urban-mobility"]
+++

The Romanian city of Cluj-Napoca has been deeply affected by urban sprawl, which has strained the city’s traffic system and caused increased congestion on roadways. The [Cal Poly Digital Tranformation Hub](https://dxhub.calpoly.edu/) (DxHub) powered by [Amazon Web Services](https://aws.amazon.com/) (AWS) collaborated with Cal Poly students and faculty from the Master of Science Quantitative Economics (MSQE) program and [World Bank](https://www.worldbank.org/en/home) members from the Geospatial Operations Support Team (GOST), and Urban, Disaster Risk Management, Resilience and Land (SCAUR), Europe and Central Asia Region team to develop a better understanding of the causes of traffic congestion throughout Cluj-Napoca. The World Bank Data Lab’s University Fellow Program and the [Development Data Partnership](https://datapartnership.org) provided the teams with access to Waze for Cities data, which enabled a novel approach to this challenge. Utilizing data from Waze for Cities, students created models which represented both temporal and spatial features of various regions in the city. These findings can be used to support policy officials in developing key infrastructure to reduce traffic congestion and inform future urban planning decisions.

## Challenge

Urbanization has greatly impacted Romania’s many growing cities, with Cluj-Napoca no exception. As the fourth most populous city in the country, Cluj-Napoca has experienced severe congestion issues due to poor infrastructure and urban sprawl in the outskirts of the city. Cluj- Napoca residents often experience severe bottlenecks when trying to move from the suburbs to the city center and drive on old roads not designed for the high density of cars in recent decades. While there is a recognition that changes need to be made to the city’s current regional planning, there is limited research examining the specific causes of traffic throughout the city.

For this challenge, students utilized Waze traffic data specific to the city of Cluj-Napoca. Waze is a free community-driven GPS navigation app that calculates road congestion by comparing current road conditions with historical road data. The data used in this study were provided through the Waze for Cities program, which included user-reported traffic alerts throughout Cluj-Napoca from 2019 through 2022. Students used the data provided as well as data scraped from various websites to create a predictive model of traffic alerts depending on a variety of covariates, such as weather, proximity to schools/hospitals, COVID-19 lockdown stringency, holidays, and more.

## Solution

This challenge was led by a group of Cal Poly MSQE students under the supervision of Dr. Carlos Flores of the MSQE program and Dr. Kurt Colvin of the Cal Poly DxHub. The students also worked closely with members of the Geospatial Operations Support Team (GOST) who provided their professional expertise and guidance throughout the collaboration. Students utilized an Amazon S3 bucket, which enabled them to store large amounts of Waze data and run various analytic models from the cloud. The students used the programming language, Python, to clean and aggregate the data, which took considerable time due to the volume of new and exploratory Waze data.

Upon completing this work, the students utilized H3, a hexagonal hierarchical spatial index that enables the analysis of geographic information. Students segmented the city into seven “parent” hexagons which contained >2000 smaller “child” hexagons and utilized both regression analysis methods and machine learning techniques (lasso and random forest models) to determine feature importance. The model utilized temporal features, which included variables such as day of the week, month, daily precipitation, daily COVID-19 Stringency Index, major Romanian holidays, and sporting events. There were also spatial features, which were the number of schools, hospitals, and bus stops in a smaller “child” hexagon.

## Impact

While there are some unique data constraints, such as the impact of COVID-19 on available data, students were able to utilize the data available to them to create robust models that displayed traffic predictors across the city of Cluj-Napoca. For instance, students determined that spatial features were greater indicators of traffic congestion near city centers whereas temporal features had a larger impact in areas closer to city outskirts. While more research and data collection are necessary, this is an exciting and critical first step in developing a greater understanding of traffic patterns in Cluj-Napoca. Hopefully future research will further develop and revise these models so they are useful in examining traffic congestion and informing policy in other cities and regions around the world.

<br>

*Originally published on the [Cal Poly Digital Tranformation Hub's](https://dxhub.calpoly.edu/) blog.*
