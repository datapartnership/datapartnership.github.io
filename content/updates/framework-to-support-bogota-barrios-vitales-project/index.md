+++
date = 2022-09-15T00:00:00Z
title = "A Data-Driven Framework to Support Bogota’s Barrios Vitales Project"
authors = ["Felipe Targa", "Ellin Ivarsson", "David Uniman", "Javier Pena"]
categories = ["Case Study"]
dev_partner = ["World Bank"]
partner= ["Waze", "Unacast", "Mapbox"]
tags = ["Transport"]
+++

A data-driven framework to support the design, implementation, and evaluation of a superblock model in Bogotá from a speed management perspective and inform street design alternatives in the urban space.

## Challenge

Throughout the world, car-dependent city planning has resulted in high levels of environmental pollution, sedentary lifestyles, traffic accidents, and increased vulnerability to the effects of climate change. These negative externalities bring to the resurgence of a new vision for sustainable cities, the “15-minute city” model that imagines places where residents can live locally, with everything they need just a short walk or bike ride away. Accordingly, Bogotá's city administration proposes a superblock model in its mobility plan, called the [Barrios Vitales (Vital Neighborhoods)](https://www.movilidadbogota.gov.co/web/barrios_vitales) project. This program seeks to transform road space currently dedicated to mobility functions by redirecting through-traffic in local streets to primary or arterial roads. Filtered permeability will incentivize unrestricted mobility for people walking and cycling while only allowing local access to motorized travel.

<figure align="center">
  <img src="/vitales_bogota_1.png"/>
  <figcaption><center> Figure 1: Selected and Potential Vital Neighborhoods in Bogotá </center> </figcaption>
</figure>

<br>

<figure align="center">
  <img src="/vitales_bogota_2.png"/>
  <figcaption><center> Figure 2: San Felipe Neighborhood with Restricted Car Use </center> </figcaption>
  <figcaption><center> Pilot Study of Vital Neighborhoods Project</center> </figcaption>
</figure>

## Solution

The World Bank Transport team combined datasets from [Unacast](https://www.unacast.com/), [Mapbox](https://www.mapbox.com/) and [Waze](https://www.waze.com/live-map/) to develop a toolkit that allows measuring [Bogota's Vital Neighborhoods initiative](https://www.movilidadbogota.gov.co/web/barrios_vitales) impact. The team assessed the traffic in the street network in a pre-intervention scenario for six selected areas scattered through Bogotá, and San Felipe was selected as the pilot neighborhood. To assess the pre-intervention scenario, the team estimated vehicle demand at the census tract scale using mobile location data from [Unacast](https://www.unacast.com/) and assigned the drivers to the available routes, using a cellular automata simulation tool that accounts for the shortest travel times and congestion. As a validation/calibration step, the team compared the estimated travel times (average speeds) in the road segments with the results of the [Mapbox Traffic Data](https://www.mapbox.com/data-products/) and with on ground collected volumes provided by Bogotá transport department. In addition, the team monitored the traffic jam length during the day, finding similar behavior between the simulated data and the Waze minute-to-minute jam reported data.

<figure align="center">
  <img src="/vitales_bogota_3.png"/>
  <figcaption><center> Figure 3: Mapbox Reported Speeds During the Day for Main San Felipe Neighborhood Roads </center></figcaption>

 <figcaption><center> <a href="https://www.mapbox.com/data-products/">Mapbox Traffic Data </a> provided by <a href="https://www.mapbox.com">© Mapbox</a>

</center> </figcaption>
</figure>

<br>

<figure align="center">
  <img src="/vitales_bogota_4.png"/>

  <figcaption><center> Figure 4: Traffic Jam Length During the Day in San Felipe Neighborhood Area. </center> </figcaption>

  <figcaption> <center> Data provided by Waze App. Learn more at <a href="https://waze.com">waze.com</a>

  </center> </figcaption>

</figure>

<br>

<figure align="center">
  <img src="/vitales_bogota_5.png"/>
  <figcaption><center> Figure 5: Through Traffic Percentage in San Felipe Neighborhood Roads</center> </figcaption>
 </center> </figcaption>
</figure>

## Impact

 The results from the traffic simulation allowed city practitioners to make informed decisions and build data-based insights to inform communities about the benefits that the project will bring to the community. As an example, it was possible to identify that, in some areas, around 75% of the vehicles congesting the local roads are through traffic (see Figure 4). In a post-intervention scenario, this toolkit will allow city planners to study and understand the role of public open space in promoting active living, road safety and public life outcomes.
