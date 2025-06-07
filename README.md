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


# Introduction

This project analyzes a dataset of large-scale power outages in the United States between 2000 and 2016. Each row represents a significant outage — defined by the U.S. Department of Energy as one that either disrupted power for over 50,000 customers or caused an unplanned demand loss of at least 300 megawatts. The dataset was compiled by Purdue University's LASCI Lab and includes geographic, climatic, and demographic context alongside each outage.

The main question driving this project is:  
**What are the important factors that causes power outages? **  
This question is relevant for improving infrastructure resilience and optimizing disaster response strategies amid climate variability.

## Dataset Summary

The dataset includes **1,534 rows**, each describing a separate outage event. The following columns are especially relevant:

| Column Name               | Description |
|---------------------------|-------------|
| `'YEAR'`                  | Year the outage occurred |
| `'MONTH'`                 | Month the outage occurred |
| `'U.S._STATE'`            | State in which the outage occurred |
| `'NERC.REGION'`           | NERC electric reliability region of the outage |
| `'CLIMATE.REGION'`        | NOAA-defined U.S. climate region |
| `'ANOMALY.LEVEL'`         | Seasonal climate anomaly (e.g., El Niño or La Niña) |
| `'OUTAGE.START.DATE'`     | Date the outage began |
| `'OUTAGE.START.TIME'`     | Time the outage began |
| `'OUTAGE.RESTORATION.DATE'` | Date power was restored |
| `'OUTAGE.RESTORATION.TIME'` | Time power was restored |
| `'CAUSE.CATEGORY'`        | Broad reason for the outage (e.g., weather, equipment failure) |
| `'OUTAGE.DURATION'`       | Length of the outage in minutes |
| `'DEMAND.LOSS.MW'`        | Peak demand lost in megawatts |
| `'CUSTOMERS.AFFECTED'`    | Number of customers without power |
| `'TOTAL.PRICE'`           | Average monthly electricity price (cents/kWh) |
| `'TOTAL.SALES'`           | Total electricity sales in MWh |
| `'TOTAL.CUSTOMERS'`       | Annual number of customers served |
| `'POPPCT_URBAN'`          | Percent of population living in urban areas |
| `'POPDEN_URBAN'`          | Urban population density |
| `'AREAPCT_URBAN'`         | Urban land area as a percentage of total state area |

These columns enable us to investigate climate-specific trends in outage severity and duration.

---
# Data Cleaning and Exploratory Data Analysis
## Data Cleaning

For this project, we worked exclusively with the `outage.xlsx` dataset, which contains detailed records of major power outages across the United States.

We began by selectThis project explores a dataset of major power outages in the United States from January 2000 to July 2016, compiled by the U.S. Department of Energy and shared by Purdue University’s LASCI Lab. Each entry in the dataset represents a significant outage event—defined as affecting at least 50,000 customers or causing a loss of 300 megawatts or more in unplanned energy demand.

Our central question is:

Are power outages that occur during cold climate conditions significantly longer than those that occur during warm conditions?

This question matters because understanding the relationship between climate conditions and outage severity can help utilities, local governments, and emergency response teams better anticipate the scale of disruption and respond proactively during different seasons.ing a subset of columns relevant to my analysis, including:

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

<iframe
  src="assets/Distribution of Outage Duration.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The distribution of outage durations is heavily right-skewed, with the majority of events lasting under 1,000 minutes, though a long tail of extreme cases exists. Most outages affect tens of thousands of customers, with some events impacting over one million. 

<iframe
  src="assets/Average Outage Duration per Cause Category.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

When looking at cause categories, outages attributed to fuel supply emergencies and severe weather have notably longer average durations, suggesting that certain causes are especially disruptive.

<iframe
  src="assets/Average Outage Duration by Cause Category Detail.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Further, detailed analysis by cause category reveals that events like coal-related outages, flooding, and transmission failures are associated with the highest average durations, exceeding 20,000 minutes in some cases. 

<iframe
  src="assets/Outage Duration vs U.S. State.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Geographic analysis by U.S. state shows a wide variation in outage lengths, with a number of states experiencing extreme values well above 50,000 minutes. Over time, average outage duration has fluctuated without a clear trend, indicating persistent structural vulnerabilities.

<iframe
  src="assets/Outage Duration Across Climate Regions.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

A boxplot by climate region also shows wide variation, especially in the Northeast and Southeast, suggesting that climate may influence outage severity.


To explore this possibility, we compared outage durations between cold and warm climates. A distribution plot shows that while both climate types have similar right-skewed shapes, cold climates appear to have slightly longer durations, particularly in the tail.

<iframe
  src="assets/Cold vs Warm Climates.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Supporting this, summary statistics show that the average and median durations in cold climates are higher than in warm climates.

| Climate Category | Number of Outages | Average Duration | Median Duration |
|------------------|-------------------|------------------|-----------------|
| cold             | 420               | 2915.30          | 1036.00         |
| normal           | 690               | 2635.97          | 726.50          |
| warm             | 280               | 2671.36          | 890.50          |


Motivated by these observations, we conducted a hypothesis test to determine whether the difference in mean outage duration between cold and warm climates is statistically significant.

---
# Assessment of Missingness
We investigated whether missing outage durations are associated with differences in the distributions of other variables: 'CLIMATE.CATEGORY', 'CAUSE.CATEGORY', and 'MONTH'. The tests were conducted using Total Variation Distance (TVD) and permutation testing to assess whether the distributions differ significantly between records with and without missing durations.

## NMAR Analysis

The column `CUSTOMERS.AFFECTED` is most likely not missing at random (NMAR) because the likelihood of missingness appears to be directly related to the unobserved value itself. In particular, outages that impact very few customers—such as small-scale or rural incidents—may be less likely to be reported or prioritized in data collection processes, increasing the chance of missing data. Conversely, during large-scale emergencies that affect a high number of customers, data may be incomplete or delayed due to logistical challenges and infrastructure disruption. In both scenarios, the probability that "Customers Affected" is missing is dependent on the actual number of customers impacted, which is not observed. This dependence on the unobserved value is a defining characteristic of NMAR, distinguishing it from missing completely at random (MCAR) or missing at random (MAR) mechanisms.



# Missingness Dependency
## 1. Climate Category
**Null Hypothesis (H₀)**: Climate category distribution is the same for missing and non-missing outage durations.

**Alternative Hypothesis (H₁)**: Climate category distribution is different for missing and non-missing outage durations.

<iframe
  src="assets/Climate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Observed TVD**: 0.0948

**Permutation p-value**: 0.0656

The p-value is slightly above the common α = 0.05 threshold, suggesting weak evidence against the null. There may be some relationship between missingness and climate category, but it is not statistically significant at the 5% level.

## 2. Cause Category
**Null Hypothesis (H₀)**: Cause category distribution is the same for missing and non-missing outage durations.

**Alternative Hypothesis (H₁)**: Cause category distribution is different for missing and non-missing outage durations.

<iframe
  src="assets/Cause.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Observed TVD**: 0.4677

**Permutation p-value**: 0.0000

The p-value is effectively zero, indicating strong evidence that the distribution of cause category is significantly different between records with and without missing durations. This suggests a non-random pattern of missingness likely tied to specific causes.

## 3. Month
**Null Hypothesis (H₀)**: Month distribution is the same for missing and non-missing outage durations.

**Alternative Hypothesis (H₁)**: Month distribution is different for missing and non-missing outage durations.

<iframe
  src="assets/Month.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

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
  src="assets/empirical.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>




# Framing a Prediction Problem
Our goal is to predict the power outage duration. This will be a regression problem because the power outage duration is measured by minutes, which is a continuous numerical variable. 

We chose this problem because by predicting power outage duration we're able to know how long different outages might last. This allows the company to assign repair crews and equipments more effectively, prioritize high-impact areas, and reduce restoration time. A prediction models generate data that can be used to improve outage prevention, infrastructure investments, and disaster readiness.

The metric we are using to evaluate my model is mean absolute error (MAE) because `OUTAGE.DURATION` contains several outliers. MAE is less sensitive to these extreme values, providing a more balanced view of average prediction error.

# Baseline Model
In this process, we build a predictive model to estimate the duration of power outages using historical outage data. We begin by selecting relevant features—`CAUSE.CATEGORY`, `ANOMALY.LEVEL`, `MONTH`, and `YEAR`—as predictors, and set `OUTAGE.DURATION` as the target variable. After removing rows with missing target values, we define preprocessing steps: numerical features (`ANOMALY.LEVEL`, `MONTH`, `YEAR`) are standardized using `StandardScaler`, while categorical features (`CAUSE.CATEGORY`) are one-hot encoded to convert them into a numerical format. These transformations are applied using a `ColumnTransformer`, which is integrated into a pipeline with a `LinearRegression` model. We split the data into training and testing sets, train the model on the training data, and evaluate its performance on the test set using three metrics: Mean Absolute Error (MAE), Root Mean Squared Error (RMSE), and R-squared (R²). This allows us to assess how well the model predicts outage duration and understand the quality of its predictions.

The predictive model achieved a Mean Absolute Error (MAE) of 2217.99 minutes, meaning that on average, the model's outage duration predictions differ from the actual values by around 37 hours. The MAE is relatively low because of the large variance in our initial distribution plot of OUTAGE.DURATION. 

# Final Model
In our final model, we used all available features in the dataset (except the target OUTAGE.DURATION) to train a Random Forest Regressor for predicting outage durations. We began by dropping any rows with missing target values to ensure model training was not affected by incomplete labels. We then automatically identified categorical and numerical features based on their data types. For numerical features, we applied a Quantile Transformer (to normalize the distribution) after imputing missing values with the median. For categorical features, missing values were filled using the most frequent value, followed by one-hot encoding to convert them into numerical format.

Next, we built a full machine learning pipeline that includes preprocessing and model training in one cohesive step. We used GridSearchCV with 5-fold cross-validation to fine-tune the Random Forest hyperparameters (n_estimators, max_depth, and min_samples_split) and evaluated model performance using mean absolute error (MAE). After training, the best model was selected based on the lowest MAE, and its performance was evaluated on the test set using MAE, Root Mean Squared Error (RMSE), and R-squared (R²). 

This round, the model performed better with a MAE of 1742.14 minutes, lowering it by 475.85 minutes. This setup allows the model to handle both missing values and feature transformations robustly, while optimizing for prediction accuracy through parameter tuning.

# Fairness Analysis
Our groups decided to use severe weather vs intentional attack outages as our fairness analysis. This is defined using the CAUSE.CATEGORY column, where one group includes all test samples labeled “severe weather” and the other includes “intentional attack.”

I decided on these groups because understanding how well the model predicts outages caused by natural events (like storms) versus mankind ones (like sabotage) is critical. These categories represent two fundamentally different causes of outages, and utilities may allocate very different types of resources to mitigate them. We want to ensure the model performs equitably across both causes so that decisions aren’t biased toward one category.

My evaluation metric is Mean Absolute Error (MAE) since I’m working with a regression model that predicts outage duration. MAE directly quantifies the average error between predicted and actual durations, making it an appropriate metric for this context.

To assess fairness, I used a permutation test with 10000 trials. I first calculated the observed absolute difference in MAE between the two groups. I then randomly shuffled the group labels across the dataset 1000 times, recalculated the MAE difference for each shuffle, and created a distribution of these values under the null hypothesis.

Null Hypothesis: The model is fair. Its MAE for severe weather and intentional attack outages is roughly the same, and any difference is due to random chance.

Alternative Hypothesis: The model is unfair. Its MAE for severe weather is significantly different from that for intentional attacks.

The p-value from the permutation test was 0.00, which is lower than the significance level. So, we reject the null hypothesis.

The figure below shows the distribution of the statistic.

<iframe
  src="assets/fairness-analysis.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
