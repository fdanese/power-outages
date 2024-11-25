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

