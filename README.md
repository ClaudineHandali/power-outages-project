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
The exploratory data analysis (EDA) reveals several important patterns in the power outage dataset. 

The distribution of outage durations is heavily right-skewed, with the majority of events lasting under 1,000 minutes, though a long tail of extreme cases exists. Most outages affect tens of thousands of customers, with some events impacting over one million. 

When looking at cause categories, outages attributed to fuel supply emergencies and severe weather have notably longer average durations, suggesting that certain causes are especially disruptive.

Further, detailed analysis by cause category reveals that events like coal-related outages, flooding, and transmission failures are associated with the highest average durations, exceeding 20,000 minutes in some cases. 

Geographic analysis by U.S. state shows a wide variation in outage lengths, with a number of states experiencing extreme values well above 50,000 minutes. Over time, average outage duration has fluctuated without a clear trend, indicating persistent structural vulnerabilities.

A boxplot by climate region also shows wide variation, especially in the Northeast and Southeast, suggesting that climate may influence outage severity.

To explore this possibility, we compared outage durations between cold and warm climates. A distribution plot shows that while both climate types have similar right-skewed shapes, cold climates appear to have slightly longer durations, particularly in the tail.

Supporting this, summary statistics show that the average and median durations in cold climates are higher than in warm climates.

Motivated by these observations, we conducted a hypothesis test to determine whether the difference in mean outage duration between cold and warm climates is statistically significant.

---
# Assessment of Missingness
We investigated whether missing outage durations are associated with differences in the distributions of other variables: 'CLIMATE.CATEGORY', 'CAUSE.CATEGORY', and 'MONTH'. The tests were conducted using Total Variation Distance (TVD) and permutation testing to assess whether the distributions differ significantly between records with and without missing durations.

## NMAR Analysis

The column `CUSTOMERS.AFFECTED` is most likely missing not at random (MNAR) because the likelihood of missingness appears to be directly related to the unobserved value itself. In particular, outages that impact very few customers—such as small-scale or rural incidents—may be less likely to be reported or prioritized in data collection processes, increasing the chance of missing data. Conversely, during large-scale emergencies that affect a high number of customers, data may be incomplete or delayed due to logistical challenges and infrastructure disruption. In both scenarios, the probability that "Customers Affected" is missing is dependent on the actual number of customers impacted, which is not observed. This dependence on the unobserved value is a defining characteristic of MNAR, distinguishing it from missing completely at random (MCAR) or missing at random (MAR) mechanisms.



# Missingness Dependency
## 1. Climate Category
**Null Hypothesis (H₀)**: Climate category distribution is the same for missing and non-missing outage durations.

**Alternative Hypothesis (H₁)**: Climate category distribution is different for missing and non-missing outage durations.

**Observed TVD**: 0.0948

**Permutation p-value**: 0.0656

The p-value is slightly above the common α = 0.05 threshold, suggesting weak evidence against the null. There may be some relationship between missingness and climate category, but it is not statistically significant at the 5% level.

## 2. Cause Category
**Null Hypothesis (H₀)**: Cause category distribution is the same for missing and non-missing outage durations.

**Alternative Hypothesis (H₁)**: Cause category distribution is different for missing and non-missing outage durations.

**Observed TVD**: 0.4677

**Permutation p-value**: 0.0000

The p-value is effectively zero, indicating strong evidence that the distribution of cause category is significantly different between records with and without missing durations. This suggests a non-random pattern of missingness likely tied to specific causes.

## 3. Month
**Null Hypothesis (H₀)**: Month distribution is the same for missing and non-missing outage durations.

**Alternative Hypothesis (H₁)**: Month distribution is different for missing and non-missing outage durations.

**Observed TVD**: 0.1432

**Permutation p-value**: 0.2122

The p-value indicates no significant evidence to reject the null. Thus, we conclude that missing durations do not appear to be related to the month in which the outage occurred.

---

# Hypothesis Testing
## Permutation Test: Mean Outage Duration by Climate Category
**Null Hypothesis (H₀)**:
The mean outage duration in cold climates is equal to that in warm climates.
(Any observed difference is due to random chance.)

**Alternative Hypothesis (H₁)**:
The mean outage duration in cold climates is different from that in warm climates.
(The observed difference reflects a true difference, not just random variation.)

**Test Statistic**:
Absolute difference in mean outage durations between the two climate categories.

**Observed Mean Difference (Cold – Warm)**:
243.93 minutes

**Permutation p-value**:
0.6270

The p-value of 0.6270 is much greater than the typical significance level of 0.05, meaning we fail to reject the null hypothesis.

**Conclusion**:
There is no statistically significant evidence to suggest that mean outage durations differ between cold and warm climate categories. The observed difference of 243.93 minutes is likely due to random chance.

<iframe
  src="assets/tvd.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>




# Framing a Prediction Problem
Our goal is to predict the power outage duration. This will be a regression problem because the power outage duration is measured by minutes, which is a continuous numerical variable. 

I chose this problem because by predicting power outage duration we're able to know how long different outages might last. This allows the company to assign repair crews and equipments more effectively, prioritize high-impact areas, and reduce restoration time. A prediction models generate data that can be used to improve outage prevention, infrastructure investments, and disaster readiness.

The metric I am using to evaluate my model is mean absolute error (MAE) because `OUTAGE.DURATION` contains several outliers. MAE is less sensitive to these extreme values, providing a more balanced view of average prediction error.
# Baseline Model
# Final Model

# Fairness Analysis
