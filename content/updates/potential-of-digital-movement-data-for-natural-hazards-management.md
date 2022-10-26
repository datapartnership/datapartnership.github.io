+++
date = 2022-10-20T00:00:00.000+00:00
title = "Where Did Everyone Go? Potential of Digital Movement Data for Natural Hazards Management"
authors = ["Nicholas Jones", "Takahiro Yabe"]
categories = ["Case Study"]
dev_partner = ["World Bank"]
partner= ["Cuebiq"]
tags = ["Disaster Risk Management"]
links = [
    "https://github.com/mindearth/mobilkit",
]
+++

Every September 19th since the great earthquake of 1985, Mexico City conducts a major drill: people filing out of buildings in an orderly fashion. But just 90 minutes after the drill finished in 2017, a new magnitude 7.1 earthquake struck causing buildings to collapse and displacing hundreds of families.

Disasters like earthquakes can strike at a moment's notice but gathering information about how they affected communities can take weeks ─ leaving humanitarian response and reconstruction financing to take place in an information vacuum.

However, the lack of information on basic facts such as post-disaster displacement may be changing thanks to new technologies. Indeed, the now-ubiquitous "location services" that allow Android or iPhone users to receive targeted weather forecasts or traffic alerts already record valuable data about movement patterns.

Taking the 2017 earthquake as a case study, recent work by our team at the [Global Facility for Disaster Reduction and Recovery (GFDRR)](https://www.gfdrr.org/en)/World Bank is uncovering valuable applications for this digital movement data and building open-source tools to bring these methods into the mainstream of disaster risk management.

### Mobility data: a generational shift

For this study, we use a dataset of about 200,000 anonymous smartphone users in Mexico who opted into sharing their GPS locations for defined purposes. The dataset was provided through the [Development Data Partnership](https) by the technology firm [Cuebiq's Data for Good](https://www.cuebiq.com/about/data-for-good/) after the application of a privacy-protection algorithm.

While the value of cellphone data in emergency response has been proven at least since the Haiti earthquake of 2010, these methods remained relatively niche and data access highly challenging.

Two factors have reshaped the landscape of mobility data. Firstly, unlike with earlier forms of phone-based movement data (Call Detail Records or CDR), users of today's smartphone apps now have explicit legal frameworks to opt in to sharing their data for a limited scope of uses under rules such as the EU's Global Data Protection Regulation (GDPR).

Secondly, COVID-19 catalyzed the rapid development of new public-private partnerships and clearer government regulations around newly available mobility datasets.

### Assessing population displacement

So how can human mobility datasets help disaster management agencies do their job? Our study documents key steps to translate a raw dataset of users' locations over time into actionable insights.

First, we divide the city into grid cells and estimate home and work locations based on the locations users visit most frequently during sleep and work hours (Figure 1).

<figure align="center">
    <img src="/images/updates/potential-of-digital-movement-data-for-natural-hazards-management/fig1.png" />
    <figcaption>
        <center> Figure 1: Estimating the density of home locations in Mexico City and adjacent regions using mean-shift
        </center>
    </figcaption>
    <figcaption>
        <center>Source: GFDRR </center>
    </figcaption>
</figure>

Secondly, we identify displacement due to the earthquake by comparing population density the night of the earthquake with its average level over the preceding two months. For example, heavily affected areas such as Cuauhtemoc district ─ the site of many building collapses ─ showed abnormally low population suggesting residents departed to shelter elsewhere.

Thirdly, we track the changes in the number of people spending the night away from their typical home census areas. In the severely affected zones, around 3.5% of the population spent the night more than 500 meters away from their homes after the quake, returning to normal over a one-month period (Figure 2).

Fourthly, we map out the destinations (within Mexico City and nationwide) to which displaced users traveled. In widely studied cases such as the New Orleans floods in the U.S. and the Great East Japan earthquake and tsunami, it is frequently observed that residents of lower-income neighborhoods which often are in hazardous areas did not move, while residents of richer neighborhoods relocated to safer zones. Following this pattern, residents of poorer neighborhoods in Mexico City stayed closer to home.

<figure align="center">
    <img src="/images/updates/potential-of-digital-movement-data-for-natural-hazards-management/fig2.png" />
    <img src="/images/updates/potential-of-digital-movement-data-for-natural-hazards-management/fig3.png" />
    <figcaption>
        <center> Figure 2: Above: Anomaly of population density during the day (left) and night (right) on
            September 19, 2017 in Mexico City. Z scores indicate the number of standard deviations more/less
            compared to the pre-earthquake mean. Below: Timeline of population displacement for heavily
            affected areas.
        </center>
    </figcaption>
    <figcaption>
        <center>Source: GFDRR </center>
    </figcaption>
</figure>

### Mainstreaming mobility insights

Beyond understanding displacement and recovery trends, our study demonstrates several uses for risk-informed urban planning ─ such as estimating daily commute patterns within a metro area and quantifying how people's access to critical services is impacted by floods. In the open source toolkit - [mobilkit](https://github.com/mindearth/mobilkit) - we provide code to automate these tasks.

Just like satellite imagery opened an increasingly wide range of uses to analysts over the past 10 years, digital movement data offers an expanding set of ways to strengthen the evidence base for response, reconstruction and risk-informed urban planning.

Such datasets can complement or replace costly surveys by understanding how people move across urban space. They can give parks authorities a picture of how far users travel to visit their facilities; and urban planners can use them to “reality check” land use schemes by grasping what time of day people visit areas zoned as residential or commercial.

Human mobility datasets are increasingly ubiquitous and power many applications on our smartphones each day. Isn’t it time to realize their public value - starting with increased ability for disaster managers to respond to unfolding emergency situations?

<figure align="center">
    <img src="/images/updates/potential-of-digital-movement-data-for-natural-hazards-management/fig4.png" />
    <figcaption>
        <center> Figure 3: (left) Histogram of distance traveled by individuals from Cuauhtemoc district, Mexico City; (right) linear relationship of displacement rate with poverty level for affected districts.
        </center>
    </figcaption>
    <figcaption>
        <center>Source: GFDRR </center>
    </figcaption>
</figure>

*Acknowledgements: The team thanks the Spanish Fund for Latin America and the Caribbean (SFLAC), Disruptive Technologies for Development (DT4D) program, USAID, Cuebiq Inc, Mind Earth, Purdue University and the Development Data Partnership.*
