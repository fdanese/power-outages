# Analysis of Power Outages
This is a project completed for DSC 80 at UCSD.

By Fred Danese

# Introduction
This project explores a dataset of major power outages across the U.S. from January 2000 to July 2016. These outages, as defined by the Department of Energy, impacted at least 50,000 customers or caused an unplanned energy demand loss of 300 Megawatts or more. The dataset was sourced from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure and provides a wealth of information, including outage details, geographical data, climate characteristics, electricity usage, and economic factors.

The central question of this project is: Where and when do major power outages tend to occur? This question is crucial as understanding the spatial and temporal patterns of outages can help utility companies and policymakers prepare for and potentially mitigate future outages. Predicting when and where outages are likely to occur can lead to improved infrastructure, better resource allocation, and more effective disaster response strategies.

The dataset contains 1,534 rows and 56 columns, but the following columns are particularly relevant to my question:

| **Column Name**           | **Description**                                                                                   |
|----------------------------|---------------------------------------------------------------------------------------------------|
| **`YEAR`**                | The year when the outage occurred.                                                               |
| **`MONTH`**               | The month when the outage occurred.                                                              |
| **`U.S._STATE`**          | The state where the outage occurred.                                                             |
| **`NERC.REGION`**         | The North American Electric Reliability Corporation region responsible for the affected area.     |
| **`CLIMATE.REGION`**      | The U.S. Climate region associated with the affected area.                                        |
| **`CAUSE.CATEGORY`**      | Broad categories describing the cause of the outage (e.g., severe weather or equipment failure).  |
| **`CAUSE.CATEGORY.DETAIL`** | Specific details about the cause of the outage within the broader `CAUSE.CATEGORY`.              |
| **`OUTAGE.DURATION.HOURS`**| The duration of the outage in hours.                                                             |
| **`CUSTOMERS.AFFECTED`**   | The number of customers affected by the outage.                                                  |
| **`DEMAND.LOSS.MW`**       | The energy demand loss caused by the outage in megawatts.                                        |
| **`OUTAGE.START`**         | The combined start date and time of the outage.                                                  |
| **`OUTAGE.RESTORATION`**   | The combined restoration date and time of the outage.                                            |

# Data Cleaning and Exploratory Data Analysis

First things first, the dataset needs to be cleaned to make it useable for our analysis.

## Cleaning

1. First, I combined the `OUTAGE.START.DATE` and `OUTAGE.START.TIME` columns into a single Timestamp object in the column `OUTAGE.START`. I performed the same combination for `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` into the column `OUTAGE.RESTORATION`. I then dropped the columns used for their combination.

2. Next, I added a column `OUTAGE.DURATION.HOURS` which converted outage duration times from minutes to hours to improve readability and make interpreting results easier.

3. Some columns such as `OUTAGE.DURATION`, `OUTAGE.DURATION.HOURS`, `CUSTOMERS.AFFECTED`, and `DEMAND.LOSS.MW` contained values of 0, which are likely to be missing values (since major outages wouldn't last 0 minutes). The value np.nan was used in place of all 0s in these columns.

Here are the first few rows of the cleaned DataFrame.

| YEAR | MONTH | U.S._STATE | NERC.REGION | CLIMATE.REGION    | CAUSE.CATEGORY   | CAUSE.CATEGORY.DETAIL | OUTAGE.DURATION.HOURS | CUSTOMERS.AFFECTED | DEMAND.LOSS.MW | OUTAGE.START       | OUTAGE.RESTORATION |
|------|-------|------------|-------------|-------------------|------------------|-----------------------|-----------------------|--------------------|-----------------|--------------------|---------------------|
| 2011 | 7.0   | Minnesota  | MRO         | East North Central | severe weather   | NaN                   | 51.000000            | 70000.0           | NaN             | 2011-07-01 17:00:00 | 2011-07-03 20:00:00 |
| 2014 | 5.0   | Minnesota  | MRO         | East North Central | intentional attack | vandalism             | 0.016667             | NaN               | NaN             | 2014-05-11 18:38:00 | 2014-05-11 18:39:00 |
| 2010 | 10.0  | Minnesota  | MRO         | East North Central | severe weather   | heavy wind            | 50.000000            | 70000.0           | NaN             | 2010-10-26 20:00:00 | 2010-10-28 22:00:00 |
| 2012 | 6.0   | Minnesota  | MRO         | East North Central | severe weather   | thunderstorm          | 42.500000            | 68200.0           | NaN             | 2012-06-19 04:30:00 | 2012-06-20 23:00:00 |
| 2015 | 7.0   | Minnesota  | MRO         | East North Central | severe weather   | NaN                   | 29.000000            | 250000.0          | 250.0           | 2015-07-18 02:00:00 | 2015-07-19 07:00:00 |

## Exploratory Data Analysis

### Univariate Analysis

I first performed some univariate analysis of the distribution of single variables to get an idea of certain patterns in the dataset.

First I checked to see what the main causes of major power outages were.

<iframe
  src="assets/Major_Outage_Causes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Then I wanted to see which climate regions experienced the most outages.

<iframe
  src="assets/Region_Outage_Count.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

I also wanted to know what the distribution of outage dirations looked like.

<iframe
  src="assets/Outage_Duration(Hours).html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

I also conducted some bivariate analysis to observe the realationships between certain variables. 

The relationship between which states have longer power outages can be insightful, so I calculated the average outage duration for each state. Here is its distribution. States like Wisconsin, Michigan, and New York are states that are geographically close together and are also at the higher end of average outage duration.

<iframe
  src="assets/AvgOutDurationByState.html"
  width="1000"
  height="600"
  frameborder="0"
></iframe>

I wanted to compare these results to the average outage duration for each climate region and see if there were some clear indicators or patterns. The east north central region had the highest average duration, falling in line with the pattern observed earlier.

<iframe
  src="assets/RegionOutageDuration.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Then I wanted to observe how the number of outages per year has changed over time to see if there are any actionable insights. There is a noteable spike in outages in 2011, which may indicate some sort of special occurence.

<iframe
  src="assets/OutagesPerYear.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Grouping and Aggregates

To gain deeper insights into the characteristics of power outages, I performed grouping and aggregation on two important aspects of the data: NERC Regions and Climate Regions.

First, I grouped the data by NERC Region and calculated the mean values for Outage Duration (Hours), Customers Affected, and Demand Loss (MW). This aggregation helps to identify the average impact of power outages in each NERC Region. The first few rows of the resulting DataFrame are shown below.

| NERC.REGION  | OUTAGE.DURATION.HOURS | CUSTOMERS.AFFECTED | DEMAND.LOSS.MW |
|--------------|------------------------|--------------------|----------------|
| ASCC         | NaN                   | 14273              | 35.0           |
| ECAR         | 93.388542             | 256354.187500      | 1314.483871    |
| FRCC         | 71.185271             | 289778.181818      | 804.450000     |
| FRCC, SERC   | 14.922222             | 126728.666667      | 466.666667     |
| HECO         | 22.783333             | 294000.000000      | 1060.000000    |


I also created a pivot table to explore the relationship between Climate Regions and Cause Categories of power outages. This pivot table displays the average duration (in hours) of outages caused by different events, grouped by climate region. It helps identify which regions are most affected by specific causes, such as severe weather. The first few rows of the resulting DataFrame are shown below.

| CAUSE.CATEGORY   | equipment failure | fuel supply emergency | intentional attack | islanding | public appeal | severe weather | system operability disruption |
|-------------------|-------------------|-----------------------|--------------------|-----------|---------------|----------------|-------------------------------|
| Central           | 5.366667         | 167.254167           | 5.767647           | 2.088889  | 23.500000     | 54.166792      | 44.920000                     |
| East North Central| 440.588889       | 566.187500           | 39.600833          | 0.016667  | 12.216667     | 73.913622      | 43.500000                     |
| Northeast         | 3.596667         | 243.826190           | 3.266412           | 14.683333 | 44.250000     | 73.831714      | 12.891667                     |
| Northwest         | 11.700000        | 0.016667             | 6.230192           | 1.222222  | 14.966667     | 80.633333      | 2.350000                      |
| South             | 4.929630         | 291.375000           | 5.427686           | 8.225000  | 19.399603     | 73.189151      | 14.434568                     |

# Assessment of Missingness

## NMAR Analysis

