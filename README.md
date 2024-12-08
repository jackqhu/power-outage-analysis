# Extreme Weather & Major U.S. Power Outages

By Jack Q. Hu (jackqhu@umich.edu)

Website: [jackqhu.github.io/power-outage-analysis](https://jackqhu.github.io/power-outage-analysis)

## Introduction

This project examines comprehensive data, used in the article entitled “A Multi-Hazard Approach to Assess Severe Weather-Induced Major Power Outage Risks in the U.S.” (Mukherjee et al., 2018), on large-scale power outage events in continental U.S. from Jan 2000 to July 2016.

The data was compiled from multiple authoritative sources, including: 

- U.S. Department of Energy (DOE)
- National Oceanic and Atmospheric Administration (NOAA)
- U.S. Department of Labor's Bureau of Labor Statistics
- U.S. Census Bureau.

As defined by the DOE, a "major outage" is classified as one that either:

1. Impacted at least 50,000 customers, or 
2. Caused an unplanned firm load loss of atleast 300MW

The scope of power outage dataset used for analysis includes information on outage: 

1. Geographic locations
2. Duration of blackout
3. Climate patterns & anomolies
4. Land & environment characteristics
5. Energy consumption patterns
6. Demographic patterns
7. Socioeconomic characteristics

My analysis is focused on answering the question: **What are the key climate and geographical factors that contribute to severe weather-induced power outages?**

**Goal of Project**: To learn weather patterns & their relationships with grid reliability/vulnerability, and build predictive models on risk of blackouts.

This is done by identifying key climate and geographical factors contributing to severe weather-induced outages to help inform infrastructure planning and risk management.

This question is consequential as weather-related disruptions continue to rise due to climate change.

Electrical grid disruptions and blackouts are catastrophic, and severely impact vulnerable populations such as those residing in:

1. Food deserts
2. Low-income communities
3. Regions with hot summers and/or cold winters.

Understanding the weather-related blackout relationship can help energy companies better prepare and manage risks, inform public policy decisions on infrastructure, and help analyze vulnerability patterns across different states & contribute to higher accuracy local-level risk maps.

## Preprocessing and Data Cleaning

1. Duration standardization:
- Converted outage durations from minutes to hours for better interpretability.
- Replaced zero values in duration and impact metrics (customers affected, demand loss) with NaN as they likely represent missing data

2. Feature engineering:
- Combined multiple urban-related metrics into a single normalized factor
- Standardized datetime fields to preserve consistency in temporal analysis

## Exploratory Data Analysis

### Severe Weather is Leading Cause of Outages

<iframe
  src="assets/outage-causes.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

| Cause                        | Count | Proportion |
|:----------------------------|------:|-----------:|
| severe weather              |   763 |       49.7 |
| intentional attack          |   418 |       27.2 |
| system operability disruption|   127 |        8.3 |
| public appeal               |    69 |        4.5 |
| equipment failure           |    60 |        3.9 |
| fuel supply emergency       |    51 |        3.3 |
| islanding                   |    46 |        3.0 |

### Geographic Distribution Heatmaps

<iframe
  src="assets/heatmap1.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

<iframe
  src="assets/heatmap2.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

States most affected by outages

| U.S._STATE  | Total Outages |
|:------------|-------------:|
| California  |          198 |
| Texas       |          122 |
| Michigan    |           95 |
| Washington  |           89 |
| New York    |           70 |

### Other Findings

<iframe
  src="assets/seasonal.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

<iframe
  src="assets/climate-region.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

<iframe
  src="assets/anomaly.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

**Summary Statistics for Weather-Related Outages:**
- Total number of weather-related outages: 763
- Average outage duration: 64.73 hours
- Median outage duration: 41.00 hours
- Average customers affected: 188,575

Univariate Analysis:
- The distribution of outage durations shows a right-skewed pattern, with most outages lasting less than 24 hours
Severe weather is the predominant cause of major power outages, accounting for a significant portion of all events


Bivariate Analysis:
- There appears to be a positive correlation between climate anomaly levels and outage duration
Different climate regions show varying susceptibility to weather-related outages, with some regions experiencing significantly more events


Interesting Aggregates:
- Climate regions with more extreme weather patterns tend to have both more frequent and longer-duration outages
The relationship between urbanization and outage frequency/duration suggests that urban areas may be more resilient to weather-related outages


Missing Data / Imputation
- Some outage durations and customer impact numbers are missing, but the pattern of missingness appears to be random rather than systematic
Climate and geographical data is generally complete, making it reliable for analysis

## Predictive Problem Framing

**Prediction Task** Our supervised ML model will focus on forecasting durations of weather-related power outages (caused severe weather events).

This is framed as a regression problem (rather than classification), as we are predicting a continuous numerical value (outage duration) rather than categorizing into discrete groups.

**Response Variable:** 
- Outage duration (in mins, will convert to hours in results section)

**Prediction-Time Available Features:**
- Climate characteristics (region, anomaly level)
- Geographic info (state, NERC region)
- Pop/urban characteristics
- Economic/infrastructure metrics 
- Temporal features (year, month)

**Evaluation Metric:** Root Mean Square Error (RMSE) as loss function for the model.
- Large penalty for large training errors; reduces model's underestimation of outage durations for inference
- Same unit as our prediction/target variable (duration in minutes); preserves interpretability

## Baseline Supervised-Learning Regression Model
Our simple model is based on a pipeline with simple linear regression, one-hot-encodng, single imputation, and minimal numerical + categorical features:
- Climate region
- Anomaly level  
- Year
- Urban factor

Performance metrics:
- RMSE: 100.3 hours
- R² Score: -0.055

The negative R² score indicates this simple model performs worse than predicting the mean duration, suggesting the need for a more sophisticated approach.

## Grid-Search CV Supervised-Learning Regression Model

This predictor model incorporates additional features and uses a Random Forest Regressor with the following improvements:

1. Additional Features:
   - Monthly seasonality
   - Economic indicators (total price, sales)
   - Infrastructure metrics (total customers)
   - Geographic granularity

2. Model Enhancements:
   - Quantile transformation of numeric features
   - Hyperparameter tuning via grid search
   - Feature engineering for seasonality

The enhanced model achieved:
- RMSE: 72.6 hours
- R² Score: 0.31

This significant improvement over the baseline suggests that incorporating additional features and using a more sophisticated algorithm better captures the complex relationships in power outage patterns. The model performs particularly well in predicting outages caused by severe weather events, which represent the majority of cases in our dataset.
