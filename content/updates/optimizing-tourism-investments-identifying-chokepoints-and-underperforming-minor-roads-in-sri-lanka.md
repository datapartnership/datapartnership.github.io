+++
date = 2020-11-09T05:00:00Z
draft = true
partner = ""
title = "Optimizing Tourism Investments: Identifying Chokepoints and Underperforming Minor Roads in Sri Lanka"
type = ""
url = ""

+++
**Challenge**: Historically, traffic data for use in road improvement planning has relied on either labor intensive or fixed sensors. While the counts can be accurate, the data themselves are limited to a single stretch of a single road and require many sensors to develop a complete picture of the road network. In a break from this labor and sensor-intensive approach, a collection of novel methods is used to combine the actual traffic speeds of vehicles with the nominal speed of a given road class. The objective of the Sri Lanka tourism pilot is to identify road segments for traffic targeting that are critical to accessing tourism hotspots throughout the country.

**Solution**: A novel method using a combination of open geospatial data, proprietary traffic data, and python modeling is used to identify traffic-based road performance and simulate improvement to travel times. 

The road targeting model was developed using Mapbox traffic data and other open source inputs.

**Impact**: Connected and maintained transportation infrastructure are critical to sustainable economic development, but most current estimation methods of road quality are labor, time, and cost intensive. This model considers the importance of 1. connectivity, 2. quality, and 3. criticality of a given road segment, while optimizing results for high utility traffic improvements. In the context of Sri Lanka, criticality is defined as the number of tourism sites that can be reached through traveling on that particular road segment.