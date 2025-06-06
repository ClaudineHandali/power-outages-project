# Power Outages Analysis
DSC 80 Final Project

By: Claudine Handali, Sharon Tey

#Introduction


# Data Cleaning and Exploratory Data Analysis
## Cleaning
# Power Outages Analysis
DSC 80 Final Project

By: Claudine Handali, Sharon Tey

---


#Introduction

---
# Data Cleaning and Exploratory Data Analysis
## 🔧 Data Cleaning

For this project, we worked exclusively with the `outage.xlsx` dataset, which contains detailed records of major power outages across the United States.

We began by selecting a subset of columns relevant to my analysis, including:

- **Temporal and geographic data:** `YEAR`, `MONTH`, `U.S._STATE`, `NERC.REGION`, `CLIMATE.REGION`
- **Outage metadata:** `CAUSE.CATEGORY`, `CAUSE.CATEGORY.DETAIL`, `OUTAGE.DURATION`, `DEMAND.LOSS.MW`, `CUSTOMERS.AFFECTED`
- **Timing information:** `OUTAGE.START.DATE`, `OUTAGE.START.TIME`, `OUTAGE.RESTORATION.DATE`, `OUTAGE.RESTORATION.TIME`
- **Climate and anomaly info:** `ANOMALY.LEVEL`, `CLIMATE.CATEGORY`
- **Economic and demographic info:** `TOTAL.PRICE`, `TOTAL.SALES`, `TOTAL.CUSTOMERS`, `POPPCT_URBAN`, `POPDEN_URBAN`, `AREAPCT_URBAN`

Next, we merged the outage date and time columns to create two unified timestamp columns: `OUTAGE.START` and `OUTAGE.RESTORATION`. These changes made it easier to calculate and visualize outage durations. I then dropped the original date and time columns.

We also addressed implausible 0 values in key outcome variables — `OUTAGE.DURATION`, `CUSTOMERS.AFFECTED`, and `DEMAND.LOSS.MW` — treating them as missing data by replacing them with `np.nan`.

Then, We created a new column called `URBAN` by combining `POPPCT_URBAN`, `POPDEN_URBAN`, and `AREAPCT_URBAN`. This new feature summarizes the extent and density of urbanization in each state to support downstream analysis.

Lastly, to prepare the <code>'CAUSE.CATEGORY.DETAIL'</code> column for analysis, I first standardized the text by stripping whitespace, converting all entries to lowercase, and ensuring that missing values were correctly treated as <code>np.nan</code>. Then, I unified variations of similar labels (e.g., <code>' coal'</code> to <code>'coal'</code>, <code>'snow/ice storm'</code> to <code>'snow/ice'</code>, <code>'wind/rain'</code> to <code>'wind'</code>) to reduce redundancy and improve consistency. This cleaning process ensures the data is more accurate and ready for grouping and statistical comparison.


With these transformations, the dataset was clean, consistent, and ready for analysis.

## Exploratory Data Analysis

### Univariate Analysis
### Bivariate Analysis
### Grouping and Aggregates

# Assessment of Missingness

# Hypothesis Testing

# Framing a Prediction Problem
# Baseline Model
# Final Model

# Fairness Analysis


## Exploratory Data Analysis
### Univariate Analysis
### Bivariate Analysis
### Grouping and Aggregates

# Assessment of Missingness

# Hypothesis Testing

# Framing a Prediction Problem
Our goal is to predict the power outage duration. This will be a regression problem because the power outage duration is measured by minutes, which is a continuous numerical variable. 
# Baseline Model
# Final Model

# Fairness Analysis
