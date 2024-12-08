# Extreme Weather and Major U.S. Power Outage Events

By: Jack Q. Hu
Email: jackqhu@umich.edu
Website: [jackqhu.github.io/power-outage-analysis](https://jackqhu.github.io/power-outage-analysis)

## Introduction
This project examines comprehensive data, used in the article entitled “A Multi-Hazard Approach to Assess Severe Weather-Induced Major Power Outage Risks in the U.S.” (Mukherjee et al., 2018), on large-scale power outage events in continental U.S. from Jan 2000 to July 2016.

The data was compiled from multiple authoritative sources, including: the U.S. Department of Energy (DOE), National Oceanic and Atmospheric Administration (NOAA), U.S. Department of Labor's Bureau of Labor Statistics, and U.S. Census Bureau.

As defined by the DOE, a "major outage" is classified as one that either:
1) impacted a population of atleast 50000
2) caused an unplanned firm load loss of atleast 300MW

The scope of power outage dataset used for analysis includes information on outage: 
1) Geographic locations
2) Duration of blackout
3) Climate patterns & anomolies
4) Land & environment characteristics
5) Energy consumption patterns
6) Demographic patterns
7) Socioeconomic characteristics

My analysis is focused on answering the question: **What are the key climate and geographical factors that contribute to severe weather-induced power outages?**

**Goal of Project**: To learn weather patterns & their relationships with grid reliability/vulnerability, and build predictive models on risk of blackouts.
This is done by identifying key climate and geographical factors contributing to severe weather-induced outages to help inform infrastructure planning and risk management.

This is consequential because weather-related disruptions continue to rise due to climate change. 
Electrical grid disruptions and blackouts are catastrophic, and severely impact vulnerable populations, particularly in:
1) Food deserts
2) Low-income communities
3) Regions with hot summers and/or cold winters.

Understanding the weather-related blackout relationship can help energy companies better prepare and manage risks, inform public policy decisions on infrastructure, and help analyze vulnerability patterns across different states & contribute to higher accuracy local-level risk maps.

## Data Cleaning and Exploratory Data Analysis

### Cleaning/Preprocessing

1. Duration standardization:
- Converted outage durations from minutes to hours for better interpretability.
- Replaced zero values in duration and impact metrics (customers affected, demand loss) with NaN as they likely represent missing data

2. Feature engineering:
- Combined multiple urban-related metrics into a single normalized factor
- Standardized datetime fields to preserve consistency in temporal analysis

