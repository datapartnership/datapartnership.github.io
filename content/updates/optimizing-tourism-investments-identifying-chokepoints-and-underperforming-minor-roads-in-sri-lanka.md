+++
date = 2020-12-01T06:00:00Z
dev_partner = ["World Bank"]
partner = ["Mapbox"]
post_type = "Article"
title = "Optimizing Tourism Investments: Identifying Chokepoints and Underperforming Minor Roads in Sri Lanka"
url = ""

+++
**Challenge**: Connected and maintained transportation infrastructure are critical to sustainable economic development, but most current estimation methods of road quality are labor, time, and cost intensive. Currently, municipalities rely on stationary approaches to obtain traffic data that are both labor and sensor-intensive. In a break from this historical approach, we rely on [Mapbox traffic telemetry data](https://www.mapbox.com/traffic-data) combined with [GOSTnets](https://github.com/worldbank/GOSTnets), a World Bank-developed open-source network analysis toolset, to identify underperforming road segments and simulate traffic reducing improvements for critical tourism hotspots across the country.

**Solution**: Using traffic data from Mapbox and other open-source tools, the World Bank Sri Lanka Tourism and Poverty Teams developed a road targeting model to calculate the shortest path and lowest cost travel between a given set of origins and destinations. The most significant finding of the model is that road quality is likely _not_ the greatest factor impacting the tourism sector in Sri Lanka. The majority of underperforming road segments were identified as trafficked or congested and associated with lighted and non-lighted intersections or entrances to tourism sites. Figure 1 indicates the geographic dispersion of the targeted road segments, colorized by the time saved for traversing each.

![](/sri-lanka.png)

_Fig 1: Map of the road segments improved by 20 seconds or more in Sri Lanka, by city origin._

Analysis indicates that there could be efficiencies gained by limited improvement to intersections, pedestrian, crossing zones or bus loading/unloading areas, which may, likewise, be impacting through traffic. On the basis of this model’s results and imagery analysis of the areas highlighted by the model, the team recommended that the Tourism project use the funds available at the tourism sites themselves and focus far less on the roads getting to them.

**Impact**: Modeling travel and traffic is an important way to quantify road connectivity issues and target investment, particularly when data are otherwise limited. This new approach represents an efficient and open-source methodology to model travel and potential traffic investment at the country level, while retaining the possibility of uncovering hitherto unidentified challenges. The model was also intentionally developed with sustainability and flexibility in mind, such that it can be re-used with improved data over time, and adapted to a variety of different contexts.

While models and novel resources are crucial to moving this type of analysis forward, it is the fusion of sophisticated models _with_ manual selection, enhanced by local information, that hold real power. This two-stage process to identify likely trouble areas allows the analyst to identify non-target slowdowns, such as intersections, and shift parameters where necessary.

_Authors:_

* Walker Kosmidou-Bradley, _Geographer, World Bank_
* Johanna Belanger, _Consultant, World Bank_
* Thomas Gertin, _Consultant, World Bank_
* Yeon Soo Kim, _Senior Economist, World Bank_