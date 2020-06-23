+++
date = ""
draft = true
partner = ""
title = "One-Stop Shop for Mobility Data: Introducing the Mobile Location Data Inventory"
type = "Article"
url = ""

+++
    As the COVID-19 pandemic proliferates, so have measures to mitigate its spread. Physical distancing and movement restrictions – such as school and workplace closures, public event cancellations, stay-at-home orders, and international and domestic travel bans – vary considerably across countries.

Since movement restrictions themselves can have profoundly negative impacts on communities, it is essential for governments to understand which policies have the most and least effective impacts.

Anonymized “mobility data” – GPS data generated primarily from smartphones – may support efforts to limit movement restriction measures to those most needed, by painting a better picture of population frequency of movement, distance of movement relative to home, daily commuting patterns, inter-district flows, and social mixing.

A number of companies have generously made these data available, either publicly or through the [Development Data Partnership](https://datapartnership.org/), to support COVID-19 response programs.

With so many new sources of information, it can be difficult to know where to start. To overcome this challenge, the Partnership, in collaboration with the World Bank Sustainable Development Data Lab and Health Global Practice, created a Mobile Location Data Inventory, which shows the availability of 17 mobility-related datasets in 90 developing economies.

Explore the data below!

    <div class='tableauPlaceholder' id='viz1592946836653' style='position: relative'><noscript><a href='#'><img alt=' ' src='https://public.tableau.com/static/images/Mo/Mobilitydataproviders_15922432451130/Summary/1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='Mobilitydataproviders_15922432451130/Summary' /><param name='tabs' value='no' /><param name='toolbar' value='no' /><param name='static_image' value='https://public.tableau.com/static/images/Mo/Mobilitydataproviders_15922432451130/Summary/1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-GB' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1592946836653');                    var vizElement = divElement.getElementsByTagName('object')[0];                    if ( divElement.offsetWidth > 800 ) { vizElement.style.width='1100px';vizElement.style.height='850px';} else if ( divElement.offsetWidth > 500 ) { vizElement.style.width='1100px';vizElement.style.height='850px';} else { vizElement.style.width='100%';vizElement.style.height='2750px';}                     var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>

#### **Apple and Google Mobility Reports**

Google launched the [Community COVID-19 Mobility Reports](https://www.google.com/covid19/mobility/) for 131 countries and regions following the outbreak. Apple released [Mobility Trends Reports](https://www.apple.com/covid19/mobility) for 63 countries, regions, and cities. Both the datasets compare the change in mobility patterns to a baseline period. While Google insights focus on movements to and from specific business types (e.g., pharmacies, retail, parks, residential), Apple insights, which are based on Apple Maps direction requests, focus on movement by mode of transportation, such as pedestrian movement and public transport.

#### **Facebook Movement Data**

The [Facebook Data for Good program](https://dataforgood.fb.com/) has released four new datasets pertaining to mobility:

• [**Movement Range**](https://dataforgood.fb.com/tools/movement-range-maps/) comprises two metrics: [Change in Movement and Stay Put](https://research.fb.com/blog/2020/06/protecting-privacy-in-facebook-mobility-data-during-the-covid-19-response/). The Change in Movement metric is an estimate of the number of the people who are “moving”, i.e., the average number of level 16 [Bing tiles](https://docs.microsoft.com/en-us/bingmaps/articles/bing-maps-tile-system) (0.6km by 0.6km) that a user was present in during an entire day versus a pre-pandemic baseline. The Stay Put metric estimates the opposite – the number of people who are staying home, i.e., percent of users that were present in only one level 16 Bing tile for at least 3 different hours on a given day. The [COVID-19 Mobility Data Network](https://www.covid19mobility.org/) provides a visualization of these [publicly available maps](https://data.humdata.org/dataset/movement-range-maps) for 14 countries.

• **Colocation Maps** presents the probability of a co-location event between users from different admin level 3 regions, predicting the rate at which people in particular towns and cities where COVID-19 cases exist and interconnect with groups of people in other places.

• **Travel Patterns** examine international travel patterns of Facebook users (who have opted into sharing location data on their mobile app), comparing users’ movement across long distances between countries, especially via air and train travel. The counts of travel patterns are updated daily. These maps can be used to understand the economic impact and recovery of the travel and tourism industries.

• **Social Connectedness Index** looks at the relative probability of friendship ties between admin level 3 or 2 or 1 region, globally. This metric may be used as a covariate layer for [predictive models for the spread of the disease](https://www.theguardian.com/world/2020/apr/14/facebook-friendships-can-help-predict-covid-19-spread-study-finds).

Although these datasets are available globally, there is a limitation of Facebook penetration.

#### **Location Data and Mobility Insights**

The Development Data Partnership has an agreement with four location intelligence companies – [Unacast](https://www.unacast.com/), [Cuebiq](https://www.cuebiq.com/),[Veraset](https://www.veraset.com/) and [X-Mode](https://www.xmode.io/). They provide near real-time mobility data, which can be used to target and design measures in response to COVID-19. The unit of measurement is a single GPS point. Utilizing such datasets, [CARTO](https://carto.com/) provides location data analysis and visualization through their platform and spatial data repository.

Mapbox offers [traffic](https://www.mapbox.com/traffic-data/?utm_medium=blog&utm_source=mapbox-blog&utm_campaign=blog%7Cmapbox-blog%7Ctelemetry%7Ctravel-changes-around-the-world-from-covid-19-cc79db7e04c7-20-03&utm_term=telemetry&utm_content=travel-changes-around-the-world-from-covid-19-cc79db7e04c7) and [telemetry density data](https://www.mapbox.com/telemetry/) for mobility analysis. As telemetry data is collected only from moving devices, changes in movement can be observed at a larger scale, especially for map improvisation and traffic prediction.

You can use the visualization below to pick which dataset suits you best based on the country you want to analyze. We are working hard to structure the data across providers in a similar way, so that you can use the same code to analyze data from all providers.

#### **Getting Access**

To get access to this data, become a development data partner by submitting proposals for new ideas.

If

_World Bank Sustainable Development Data Lab is a community of practice for the Bank staff seeking to modernize project data practices through learning, networking, and shared resources._

_The Development Data Partnership is a partnership between international organizations (Development Partners) and companies (Data Partners), created to facilitate the use of third-part data in research and international development._