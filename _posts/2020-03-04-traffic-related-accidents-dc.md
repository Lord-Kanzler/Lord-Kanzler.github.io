---
layout: post
title: The Risk of Injury and Death Roaming the Streets of Washington DC 
subtitle: Traffic related Injuries and Deaths in DC since JAN-01-2019
gh-repo: Lord-Kanzler/DS-Unit-1-Build
gh-badge: [star, fork, follow]
tags: [DS, DC, Traffic]
comments: true
---

I’m an avid cyclist and motorcycle rider, and since I moved to the Washington DC area a couple of years back, people kept on asking me “if I’m not even a tiny bit afraid riding around in DC.” Usually, I didn’t think too much of it afterward thinking “well traffic is bad in any major city in the US,” and continued doing what I like, and maybe ignoring my inner voice of reason. That is until I was hit by an inattentive driver texting on his phone.

The accident was only minor, a couple scuffs and bruises, nothing serious. And don’t get me wrong, I am highly aware of the bad traffic reputation the DC metro area has, and I am on my guard at all times.

But ever since, I have been wondering if there are specific areas cyclists or bikers should avoid, or if there are areas particularly well suited for roaming the streets on two wheels.

For the purpose of this investigation, I have aquired a data set on all reported motor vehicle related incidents, which is maintained by the District Department of Transportation (DDOT). This data can be accessed via the [Open Data DC](https://opendata.dc.gov/datasets/70392a096a8e431381f1f692aaa06afd_24) repository. 

{% include gmap_plot_all_2020.html %}

*Figure Legend: BLUE CIRCLES indicate Motor Vehicle accidents involving Cyclists. GREEN CIRCLES indicate Motor Vehicle accidents involving Pedestrians. RED CIRCLES indicate Motor Vehicle only accidents. Variation is Circle Size indicates the number of impaired individuals*

I started out creating a visualization showing occurences of traffic related incidents resulting in injuries or deaths, trying to determine if there are some localized trends for for motor vehicle accidents involving cyclists, pedestrians, and car collisions. 

While informative and somewhat staggering, these incidents appear to be of a systemic nature, rather than follow localized patterns. Since the beginning of 2020 alone, there were 4061 reported incidents resulting in some form of injuries or death. 

Realizing that the rate of incidents is far larger than imagined, I started looking into accidents with fatal outcomes. For this purpose I have chosen to include 2019 data.

{% include gmap_plot_fatal_2019.html %}{: .center-block :}

With 30860 reported incidents since Jan 1st 2019 the total number of accidents still seems rather large in relation to the size of Washington DC, it is not as bad as first impressions suggested. Within this timeframe, there were 33 traffic related fatalities. Of wich roughly 80% involved car crashes or car on pedestrian accidents. The smallest fraction involved cyclists.

{% include plotly_injury_and_death_2019.html %}{: .center-block :}








If interested in in the Code Source, please click [here](https://github.com/Lord-Kanzler/DS-Unit-1-Build/blob/master/LS_DS13_Unit_1_Build_DATA_ALEX_KAISER.ipynb) for the Google Colabs Notebook.
