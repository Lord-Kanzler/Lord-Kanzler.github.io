---
layout: post
title: Developing a Bridge Site Visualization and Prediction Tool for the Bridges to Prosperity Organization
subtitle: Or an extensive exercise in data cleaning and preparation
gh-repo: Lambda-School-Labs/Labs25-Bridges_to_Prosperity-TeamB-ds
gh-badge: [star, fork, follow]
image: /img/B2P.jpg
tags: [Python, Models, Data Science, AWS Elastic Beanstalk, Docker, FastAPI]
comments: false
---

Currently I am going through the Labs section as a part of Lambda Schools curriculum, and for this purpose we have partnered with [Bridges To Prosperity](https://bridgestoprosperity.org/)

Bridges to Prosperity is a nonprofit organization that helps build footbridges in east african communities such as Rwanda. They have collected lots of data about various Rwandan villages and bridge sites, and have approached as with a request to help with building a tool allowing for the visualization of current and future bridge sites, with the inclusion of a predictive model determining and assess potential sites with a high social-economic impact on rural communities in the region.

We have been provided with extensive data sets on site assessments, as well as Rwandan administrative data, but due to the countries tumultuous recent history, in which it underwent substantial changes in it's administrative organization, many village names, districts and sectors have been renamed or have changed their affiliations, resulting in substantial mismatches of information across varying sets of data. This is especially compounded, by the fact that Rwanda is only sparsely mapped, requiring Bridges to Prosperity members to collect their data while having "boots on the ground" and all those unavoidable inconsistencies that find their way into the data collection process.   

For the purpose of this project, our cross-functional team of 5 web-developers and 3 data scientists(including myself) have been tasked with the creation of mapping tool allowing for easy and quick visualization of rwandan bridge construction projects and the display of information regarding their respective statuses and meta information of their impacts on local communities they serve.

We also have been tasked with the development of a predictive model on future bridge site locations, factoring in social, economical and medical impacts, to allow B2P to plan and advocate for the construction of bridges in such critical locations. 


Going into this project I knew that I am working with a capable team of developers and had no doubt that the requested visualization and prediction tool could be build. However, as a data scientist I know that at the core of such an endeavour lies the need for a thorough and meticulous prepared source data, which turned into the emphasis of this entire project.

During initial discussion with the entire project team and our stakeholders, we decided that data science team members will focus predominantly on data preparation, and the web-development team will be building the a mapping tool and handle any necessary visualization aspects with the aim of producing a more dynamic and intuitive visualization tool.  

A full breakdown of the Project Roadmap can be found [here](https://www.notion.so/Bridges-to-Prosperity-Roadmap-Student-Facing-2ba09fe5039e4e78931f85a282fb8617).

After an initial breakdown of tasks among the team, me and my fellow data scientists colleagues consulted on further breaking down and assigning the individual tasks.

- Initial data exploration to get an overview of the situation

- Cleaning of naming and labeling inconsistencies 

- Necessary translations across individual sets of data

- Removal of formatting and spelling artifacts


### Division of tasks and technical challenges

The majority of this projects work load for **RELEASE 1** was and is centered around data preparation, data cleaning, and merging procedures. While this may sounds trivial at first, there were quite a few challenges to overcome considering the state of the data provided to us.

I won't go into the nitty-gritty of it, as this wouldn't serve anyone, and the intricacies of thorough data cleaning and reconstruction procedures only rarely rouse anyone's excitement. Just know, it is a lot of work, and it can be difficult!

Anyhow, I initially started out with data exploration to assess the situation, providing summaries to my colleagues. But shortly after they took over large aspects of data cleaning procedures, due to the need to keep procedures consistent, and to many cooks tend to spoil the broth ;) 

As a result I assumed the role of a dev-ops engineer setting up our Data API framework and created AWS hosted data endpoints, forwarding relevant data to our web-dev team members. 

Throughout our training, we only rarely operate on a cross-functional basis, so my experience in working with JSON objects, and some field specific terminology differences posed the majority of technical differences I have encountered. 

Initial data endpoints served as static data dumps to web-backend developers, aiding in setting up SQL databases on their end.

The API framework is based on the FAST-API framework, initially deployed in Docker containers, and subsequently hosted online via AWSs Elastic Beanstalk framework.

Usually these simple 1-to-1 data dump endpoints look something like this:

```py
# Package imports
from fastapi import APIRouter
import pandas as pd
import json

router = APIRouter()

# Loading Dataset
names_codes = "https://raw.githubusercontent.com/Lambda-School-Labs/Labs25-Bridges_to_Prosperity-TeamB-ds/main/data/edit/Rwanda_Administrative_Levels_and_Codes_Province_through_Village_clean_2020-08-25.csv"

# Converting to dataframe using pythons Pandas library
names_codes = pd.read_csv(names_codes)

# defining a function to set up the /villages endpoint
@router.get("/villages")
async def villages():
    output = names_codes.to_json(orient="records")
    parsed = json.loads(output)
    return parsed

``````
The resulting endpoint output looks like this: (Here a [link](http://bridges-to-presperity-08272020.eba-3nqy3zpc.us-east-1.elasticbeanstalk.com/villages) if in case you want to take a look)

```py

  {
    "Unnamed: 0": 0,
    "Prov_ID": 2,
    "Province": "Southern Province",
    "Dist_ID": 22,
    "District": "Gisagara",
    "Sect_ID": 2205,
    "Sector": "Kigembe",
    "Cell_ID": 220501,
    "Cell": "Agahabwa",
    "Vill_ID": 22050101,
    "Village": "Agahehe",
    "Status": "Rural",
    "FID": 1842
  },
  {
    "Unnamed: 0": 1,
    "Prov_ID": 2,
    "Province": "Southern Province",
    "Dist_ID": 22,
    "District": "Gisagara",
    "Sect_ID": 2205,
    "Sector": "Kigembe",
    "Cell_ID": 220501,
    "Cell": "Agahabwa",
    "Vill_ID": 22050102,
    "Village": "Kabacuzi",
    "Status": "Rural",
    "FID": 1846
  },...

```

Not really much more to it.


### Current progress, 