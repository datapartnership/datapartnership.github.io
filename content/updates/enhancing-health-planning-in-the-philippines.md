+++
date = 2022-10-24T00:00:00.000+00:00
title = "Enhancing Health Planning in the Philippines"
authors = ["Andres Chamorro"]
categories = ["Case Study"]
dev_partner = ["World Bank"]
partner= ["Mapbox"]
tags = ["Health", "Urban Developement"]
links = [
    "https://docs.mapbox.com/api/navigation/matrix",
]
+++

## Challenge

The delivery of health services in the Philippines is highly fragmented across different levels of administration. Health systems are autonomously run by local government units (LGUs), whether they are barangays (the most local level), cities, municipalities, or provinces. Historically, these systems have been poorly coordinated, leading to overcrowded hospitals and clinics in some locations and different health outcomes across regions.

Actions are underway to promote a more integrated approach in service delivery. The government has mandated the formation of province and city-wide healthcare provider networks. These systems shall integrate all public facilities and coordinate referrals across different levels of care, from local health stations to rural clinics, to primary and emergency care. The [World Bank Health Global Practice](https://www.worldbank.org/en/topic/health) was asked to advise on the design of these systems, starting with a comprehensive mapping of existing health facilities, and technical analysis to identify catchment areas and referral pathways.

## Solution

The team conducted geospatial analysis to advise on key questions that could inform a more integrated health care system, starting with three LGUs (Bohol, Baguio City, and Maguindanao).

1. Where are the existing health care facilities?

    First, we mapped all the health sites from the [Department of Health](https://doh.gov.ph)’s official registry, geocoding the facilities with missing location.

2. How well are people connected to different health services?

    We used the [Mapbox Matrix API](https://docs.mapbox.com/api/navigation/matrix/) to calculate people’s access to health services. The API returns travel times from origin and destination pairs, according to different modes of transportation (for example, driving with traffic or walking). In our assessment, we used the API to record the time it takes to travel from every 1km grid cell in the region to the nearest health clinic or hospital. This method is a big improvement over estimating access with straight distances. The flexibility of the API with regards to transport modes proved to yield more realistic travel time estimates than traditional GIS methods (road network analysis or friction surface).

<figure align="center">
    <img src="/images/updates/enhancing-health-planning-in-the-philippines/fig1.jpg"  width="600"/>
    <figcaption>
        <center> Figure 1: Accessilibity based on the Mapbox Matrix API in Baguio City, PH. Source: Project team. </center>
    </figcaption>
</figure>

3. How are health facilities connected to each other?

    We classified health facilities according to different service levels: tier 3 (barangay health stations), tier 2 (rural health units, municipal health offices, birthing homes), tier 1 (hospitals, infirmaries). Then we used the same method of travel time analysis to determine networks between facilities from each tier. These networks were visualized in maps and presented as an Excel table that preserved the nested hierarchy of the networks, with additional attributes on population catchment and capacity.​

<figure align="center">
    <img src="/images/updates/enhancing-health-planning-in-the-philippines/fig2.jpg" />
    <figcaption>
        <center> Figure 2: Health facilities network in Bohol, PH.</center>
    </figcaption>
    <figcaption>
        <center>Source: Analysis by study team using the Mapbox Matrix API</center>
    </figcaption>
</figure>

## Impact

The maps and tables were presented to staff from the health ministry for the three local government units.

<figure align="center">
    <img src="/images/updates/enhancing-health-planning-in-the-philippines/fig3.png" />
    <figcaption>
        <center> Figure 3: Health facilities network in Maguindanao, PH.</center>
    </figcaption>
    <figcaption>
        <center>Source: Analysis by study team using the Mapbox Matrix API</center>
    </figcaption>
</figure>

<figure align="center">
    <img src="/images/updates/enhancing-health-planning-in-the-philippines/fig4.png" />
    <figcaption>
        <center> Figure 4: Health facilities network in Baguio, PH.</center>
    </figcaption>
    <figcaption>
        <center>Source: Analysis by study team using the Mapbox Matrix API</center>
    </figcaption>
</figure>

The work was well received and helped frame discussions around optimal ways to register citizens to health care providers. Should referrals be based solely on proximity and travel time? The methodology presented cannot be taken as a golden solution as there are multiple factors that affect efficiency and equity in health care delivery. Still, the initial mapping is a valuable starting point, and hopefully, the first of many analyses conducted to prepare a more integrated health care system in the Philippines.

*Acknowledgements: The team thanks Mapbox and the Development Data Partnership.*
