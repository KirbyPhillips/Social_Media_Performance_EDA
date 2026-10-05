# Social_Media_Performance_EDA

### Note:

This repository outlines the data science process used to analyse social media performance.

It covers exploratory data analysis, data quality assessment, and deeper analysis of content, post types, platforms, and timing. Key findings and potential business implications are presented throughout the project.

## Table of Contents

This repository presents the process used to analyse social media performance, from data inspection and exploratory analysis through to key findings, business impact, and potential future improvements.

1. [Project Overview](#project-overview)
2. [Data Inspection](#data-inspection)
3. [EDA Health Check](#eda-health-check)
4. [Descriptive Statistics](#descriptive-statistics)
5. [Missing Values](#missing-values)
6. [Univariate Analysis](#univariate-analysis)
7. [Bivariate Analysis](#bivariate-analysis)
8. [Correlation & Heatmap](#correlation--heatmap)
9. [Outlier Detection](#outlier-detection)
10. [Time Series EDA](#time-series-eda)
11. [Categorical Deep Dive](#categorical-deep-dive)
12. [Key Findings & Business Impact](#key-findings--business-impact)
13. [In Hindsight](#in-hindsight)
14. [Concluding Notes](#concluding-notes)
    - [Tools & Technologies](#tools--technologies)
    - [Technical Skills Demonstrated](#technical-skills-demonstrated)
---

## 1. Project Overview

This project analysed 5,600 social media posts across 6 platforms to understand which factors are associated with stronger engagement and where the business should focus its content strategy.

The analysis investigates:

- Which content factors are associated with higher engagement rates?
- Does posting day or time meaningfully affect engagement?
- Which metrics provide the most useful measures of content performance?

The project addressed these questions through exploratory data analysis, statistical analysis, data visualisation, and business-focused interpretation.

---

## 2. Data Inspection

This dataset was inspected to determine its **size, structure, data types, and missing values** before conducting any analysis. This helps establish whether the data is suitable for analysis and highlights any limitations that could affect business conclusions.

- `df.shape` shows the number of rows and columns.
- `df.info()` shows column names, data types, and non-null counts.
- `audit()` provides a deeper column-level check of nulls, unique values, and sample values.
- Memory usage confirms the dataset is lightweight enough to work with efficiently.

### The data inspection revealed that:

- The dataset contains **5,600 posts across 24 columns**.
- Only **Clicks** and **Click_Through_Rate** contain missing values, with **3,740 missing values (66.8%)** each.
- The missing click data appears **structural rather than random,** likely relating to specific platforms and post types that do not generate trackable clicks.
- Data types are clean and logical: strings are stored as objects, counts as integers, rates as floats, and dates as datetime.
- The dataset is lightweight at just over 1MB, so it can be processed efficiently throughout the analysis.

### Why is this important to the business?

The data inspection shows that the dataset is generally well structured for analysing social media performance, but click-based metrics have a significant limitation. With click data available for only part of the dataset, the business should be cautious when using Clicks or CTR to compare overall content performance. This limitation needs to be considered before making decisions based on those metrics.

---

## 3. EDA Health Check

This EDA Health Check was run across the whole dataset to review data quality, missing values, duplicates, distributions, and category structure before deeper analysis. It provides an overall view of the dataset and highlights any issues that could affect how the results should be interpreted.

### The EDA Health Check revealed that:

- The dataset has **5,600 rows and 24 columns**, with **no duplicates** and only **Clicks** and **Click_Through_Rate** missing (66.79% each).
- Engagement and other performance metrics are **strongly right-skewed**, with a small number of high-performing posts pulling the mean above the median.
- The dataset contains **6 platforms, 5 content categories, 7 post types, and 8 regions**; Video is the dominant post type and Educational is the largest content category.
- **Nine numeric columns are highly skewed**, so median performance is more representative than the mean and high-performing outliers should be investigated rather than removed.
- The **66.79% missing rate for click data** limits click-based analysis to **1,860 posts** and should be treated as a key data limitation.

### Why is this important to the business?

The health check confirms that the data can be used to identify meaningful patterns in social media performance, while highlighting important limitations. Using median performance alongside the mean helps provide a more realistic view of typical content performance, while the missing click data limits how confidently the business can assess click-based performance.

---

## 4. Descriptive Statistics

The descriptive statistics summarise the main characteristics of the dataset, including typical performance, variation, and relationships between key metrics. They help establish how performance should be measured and interpreted before moving into deeper analysis, and it includes:

- Calculates summary statistics for the dataset, including **mean, median, skewness, kurtosis, and null counts** across key metrics.
- Separates columns into **numeric, categorical, and datetime** types to understand the structure of the data.
- Produces a **correlation matrix** to identify relationships between numeric variables and guide dashboard metrics.

### The descriptive statistics summary revealed that:

- The dataset contains **14 numeric, 8 categorical, and 2 datetime columns**, supporting both performance and time-based analysis.
- **Engagement, Impressions, Views, Likes, Shares, and Comments are strongly right-skewed**, making the median more representative of typical performance than the mean.
- **Engagement_Rate and Click_Through_Rate are more consistent**, with mean and median both around 0.15 and 0.02 respectively; however, CTR is limited by **3,740 missing values**.
- **Engagement strongly correlates with Views and Impressions (0.90)**, while Likes, Shares, Comments, and Clicks also move closely together. In contrast, **Engagement_Rate negatively correlates with reach metrics**, showing that high reach does not necessarily mean high engagement rate.
- The correlation results support keeping **total Engagement and Engagement_Rate as separate dashboard metrics**, while Post_Hour and geographic coordinates show little relationship with performance.

### Why is this important to the business?

These statistics help the business distinguish between typical performance, unusually high performance, and metrics that provide different types of insights. This supports more reliable performance reporting and helps avoid treating highly correlated or redundant metrics as separate measures.

---

## 5. Missing Values

This section examines the amount and pattern of missing data to understand whether missing values could affect the reliability of the performance analysis. It also identifies where missing data is concentrated and whether it represents a limitation for specific metrics. It:

- Measures and visualises missing data by calculating **null counts and percentages** for each column.
- Identifies columns exceeding a **30% missing-value threshold** and investigates missing Clicks and Click_Through_Rate by **Platform and Post_Type**.
- Uses a **bar chart** to understand the pattern and concentration of missing data.

![Alt text](images/Missing%20Values%20Code.png)
*Python code used to analyse missing values and generate the visualisations.*

![Alt text](images/Missing%20Value%20Pattern.png)

### The missing values investigation revealed that:

- Only **Clicks** and **Click_Through_Rate** contain missing values, with **3,740 missing values each**; all other columns are complete.
- The heatmap shows that the missing values are highly concentrated in these two columns rather than being distributed randomly across the dataset.
- The bar chart confirms that both Clicks and Click_Through_Rate have the same number of missing values.
- This concentrated pattern indicates that the missingness is structural rather than widespread across the dataset.

### Why is this important to the business?

The missing click data limits how confidently the business can use Clicks and Click_Through_Rate to compare content performance. Recognising this limitation helps prevent incomplete data from leading to misleading performance conclusions.

---

## 6. Univariate Analysis

This univariate analysis examines individual variables to understand their distribution, spread, and frequency before comparing relationships between variables. It helps identify patterns such as skewness and extreme values that could affect how performance is interpreted. It:

- Examines each variable individually using **histograms, boxplots, QQ plots, and categorical bar charts**.
- Assesses the **distribution, spread, outliers, and category frequencies** before analysing relationships between variables.

![Alt text](images/Engagement%20Distribution.png)

### The univariate analysis revealed that:

- Engagement is strongly right-skewed, with a mean of 115,444.90 compared with a median of 58,991.50, indicating that a smaller number of high-performing posts pull the average upward.
- The boxplot shows numerous high-value outliers, confirming that engagement volume varies substantially across posts.
- The QQ plot shows a clear departure from normality, particularly in the upper tail, reinforcing the strong right-skewed distributions.
- Engagement has no missing values, with 0 nulls shown in the summary statistics.
- Given the skewed distribution and presence of extreme values, the median provides a more representative measure of typical Engagement than the mean.

### Why is this important to the business?

Understanding the typical level of engagement helps the business set more realistic expectations for content performance and avoid letting a small number of exceptionally high-performing posts distort the overall picture.

---

## 7. Bivariate Analysis

This bivariate analysis examines how Engagement_Rate differs across content categories to identify which types of content are associated with stronger performance. Comparing these categories helps show whether content choice is linked to meaningful differences in engagement. It:

- Examines relationships between pairs of variables using **boxplots, violin plots, group-level statistics, and cross-tabulations**.
- Compares Engagement_Rate across Content_Category to identify differences in performance between content types.
- Identifies which content categories are associated with different levels of engagement performance.

![Alt text](images/Engagement%20Rate%20by%20Content%20Category.png)

### The bivariate analysis revealed that:

- **Customer Story and Educational** content show the highest typical Engagement_Rate, with both having a median of approximately 0.20.
- **Entertainment** has the lowest typical Engagement_Rate, with a median of approximately 0.08.
- **Event / Webinar and Product Promotion** fall between these categories, with median Engagement_Rates of approximately 0.14 and 0.12 respectively.
- The distributions also show that **Product Promotion** contains several higher-value observations, visible as individual points above the upper whisker.
- Overall, the graph shows **meaningful differences in Engagement_Rate across Content_Category,** with Educational and Customer Story content associated with substantially higher typical engagement than Entertainment content.

### Why is this important to the business?

The analysis shows that content choice is associated with meaningful differences in engagement, giving the business a clearer basis for evaluating which types of content perform more strongly.

---

## 8. Correlation & Heatmap

This correlation analysis examines how the main performance metrics relate to one another and whether they provide similar or different information. It helps identify which metrics move together and which provide distinct insights into performance, and it:

- Calculates the **correlation between every pair of numeric metrics** and visualises the results using a colour-coded heatmap.
- Masks the upper triangle to remove duplicate values, leaving each unique correlation pair visible once.
- Extracts and ranks the **top 15 most correlated pairs** by absolute correlation strength.
- Uses correlation analysis to identify which metrics **move together and which measure different behaviour**.
- Supports dashboard design by identifying metrics that are redundant versus those that provide distinct performance insights.

![Alt text](images/Correlation%20Matrix.png)

### The correlation check revealed that:

- **Engagement, Impressions, Likes, Shares, Comments, Views, and Clicks form a strong positive correlation cluster**, with correlations ranging from **0.80 to 1.00**, indicating that these volume metrics generally move together.
- **Engagement_Rate behaves differently**, showing weak-to-moderate negative correlations with reach and interaction metrics, including **-0.32 with Impressions and Views**, and a near-zero correlation of **0.04 with Engagement**.
- **Impressions and Views have a perfect correlation of 1.00**, indicating that they are essentially measuring the same metric in this dataset.
- **Likes, Shares, Comments, and Clicks correlate strongly with Impressions and Views**, with correlations generally between **0.87 and 0.95.** 
- Overall, the results support treating **Engagement and Engagement_Rate** as distinct performance measures: Engagement reflects interaction volume, while Engagement_Rate captures the proportion of the audience that interacted.

### Why is this important to the business?

Understanding these relationships helps the business focus on metrics that provide distinct insights and avoid treating highly correlated metrics as separate measures of performance.

---

## 9. Outlier Detection

The outlier analysis identifies unusually high or low performance values and examines whether they represent genuine variation in social media performance. It helps distinguish potentially meaningful high-performing posts from values that could distort the overall analysis. It specifically:

- Identifies unusually high or low values using **IQR and Z-score methods**.
- IQR is more suitable for **skewed data**, while Z-score identifies more extreme deviations from the mean.
- Flags outliers with an **Is_Outlier** column rather than removing them, allowing them to be included or excluded during analysis.

![Alt text](images/Outlier%20Overview.png)

### The outlier detection revealed that:

- **Engagement has the highest IQR outlier count**, with 435 posts (7.8%), followed by Comments (7.1%), Likes (5.5%), and Shares (5.2%).
- **Engagement_Rate has zero IQR outliers**, reinforcing that it is a stable and consistent performance metric.
- The Z-score method flags **263 rows**, identifying a smaller set of more extreme observations.
- The boxplots show that **Engagement, Likes, Shares, Comments, Impressions, Views, and Clicks** contain numerous high-value observations beyond the upper whiskers, reflecting the strongly right-skewed nature of these metrics.
- These observations should not automatically be treated as errors. They may represent legitimate high-performing posts, so retaining and flagging them allows the analysis to account for their influence without unnecessarily removing valid data.

### Why is this important to the business?

Identifying high-performing outliers helps the business understand exceptional content performance without allowing it to distort typical results. Retaining these posts also provides an opportunity to investigate what may have contributed to their success.

---

## 10. Time Series EDA

This EDA examines how social media performance changes over time, including monthly engagement, day of week, and posting hour. It helps determine whether timing is meaningfully associated with performance and whether there are notable changes in engagement over the analysis period. It specifically:

- Analyses performance patterns across **monthly, weekly, and hourly time dimensions**.
- Converts `Post_Date` into a proper datetime format and groups **total engagement by month** to identify overall trends.
- Extracts the **day of week** from each post date to compare average engagement rates.
- Groups posts by **posting hour** to identify patterns in engagement performance.
- Assesses whether posting time is associated with differences in engagement performance.

![Alt text](images/Monthly%20Total%20Engagement%20Trend.png)

### The time series EDA revealed that:

- **Monthly total engagement remains relatively stable** from January 2024 through approximately April 2025, generally ranging between **35 million and 45 million per month**, with normal month-to-month fluctuations.
- The sharp decline in **May 2025** is caused by the dataset containing only part of the month; this period should therefore be excluded or flagged when reporting trends.

### Why is this important to the business?

The relatively stable trend suggests that overall engagement has remained consistent over the period analysed, while the partial May 2025 data should not be used to draw conclusions about a genuine decline in performance.

---

## 11. Categorical Deep Dive

The categorical deep dive compares platforms, post types, and content categories to understand how engagement performance differs across the main content dimensions. It helps identify which categories provide the most meaningful opportunities for comparing and improving content performance. It specifically:

- Examines each **categorical variable** by checking unique values, the most common category, percentage share, and potential imbalance.
- Flags categories where a single value represents more than **90% of rows** to assess whether segmentation is meaningful.
- Calculates average **Engagement_Rate** for each value within Platform, Post_Type, and Content_Category.
- Consolidates the key categorical findings into a **ranked summary** of engagement performance.
- Identifies which **platform, post type, and content category** are most associated with higher engagement rates.

### The categorical deep dive revealed that:

- No categorical variable is highly imbalanced, with no single value exceeding the **90% threshold**; Video is the most common post type at **52.6%** but remains suitable for comparison.
- **Instagram leads platform performance at 0.16**, while LinkedIn is lowest at 0.14; the **0.02 range** makes platform the weakest differentiator.
- **Image and PDF posts lead at 0.18**, while Video is lowest at 0.14 despite representing **52.6% of all posts**; the **0.04 range** is twice that of platforms.
- **Content Category shows the strongest variation**, with Educational and Customer Story at **0.20** and Entertainment at **0.08**, producing a **0.12 range**.
- The findings indicate that **content category is the strongest differentiator, followed by post type and then platform**, making category-level performance an important focus for the dashboard and portfolio analysis.

### Why is this important to the business?

The analysis shows that content category provides the clearest differentiation in engagement performance, giving the business a stronger basis for evaluating content strategy and where further performance analysis should focus.

---

## 12. Key Findings & Business Impact

### Key Findings

1. **Content Category shows the strongest association with engagement.**
   Educational and Customer Story content achieve the highest Engagement_Rate at **0.20,** compared with **0.08** for Entertainment. Content Category shows a larger difference in performance than Post Type or Platform.

2. **Posting time does not meaningfully differentiate engagement.**
   Average Engagement_Rate is approximately **0.15 across all days,** while posting hours range only from **0.148 to 0.158.** The differences are small compared with those observed across content categories and post types.

3. **Engagement volume and Engagement_Rate provide different performance insights.**
   Engagement is strongly related to reach and interaction volume, while Engagement_Rate measures a different aspect of performance. This means both metrics can provide useful but distinct views of content performance.

4. **Impressions and Views are redundant as separate performance metrics in this dataset.**
   Impressions and Views have a perfect correlation of 1.00 in this dataset, suggesting that including both as separate dashboard metrics would provide little additional information.

5. **Click-based performance has an important data limitation.**
   Clicks and Click_Through_Rate are missing for 3,740 posts (66.8%), meaning click-based analysis is limited to 1,860 posts and should be interpreted with caution.

---

### Business Impact & Financial Value

This EDA identifies potential business value and areas for further investigation; financial impact was not directly measured because the dataset does not contain the required commercial data.

1. **Content Strategy:**
   - The **0.12 Engagement_Rate gap** between the highest and lowest content categories provides a measurable basis for evaluating content strategy.
   - Financial value could be assessed by connecting engagement performance with **clicks, leads, conversions, CAC, and revenue.**

3. **Content Scheduling:**
   - The limited variation in Engagement_Rate across days and hours suggests **minimal performance differences from posting time.**
   - Financial value could be assessed through **content costs, cost per engagement, and campaign ROI.**

5. **Performance Measurement:**
   - Separating **Engagement** from **Engagement_Rate** and removing redundant metrics creates a clearer performance measurement framework.
   - Financial value could be assessed through **CTR, leads, conversions, revenue, ROAS, and marketing ROI.**
---

## 13. In Hindsight

This analysis identified meaningful differences in engagement across content, post type, platform, and timing. A future iteration could connect these findings to business outcomes.

### Data & Analysis

- Add clicks, leads, conversions, and revenue to measure commercial impact.
- Analyse performance across platforms and audience segments.

### Dashboard & Business Application

- Develop an interactive dashboard focused on content performance and business KPIs.
- Measure the impact on engagement, conversions, and marketing ROI.

A future iteration would therefore move beyond analysing engagement to measuring its direct contribution to business performance.

---

## 14. Concluding Notes

This concludes the analysis of social media performance across 5,600 posts and 6 platforms. The project demonstrates how exploratory data analysis can identify meaningful performance patterns and translate them into business-focused insights.

### Tools & Technologies

| Tool | Purpose |
|---|---|
| **Python** | Data analysis and visualisation |
| **Pandas** | Data manipulation and preparation |
| **Matplotlib & Seaborn** | Data visualisation |
| **NumPy** | Numerical analysis |
| **Jupyter Notebook** | Analysis workflow |

---

### Technical Skills Demonstrated

**Data Analysis & Preparation**
- Python
- Pandas
- Exploratory Data Analysis
- Data Quality Assessment
- Missing-Value Analysis

**Statistical Analysis**
- Descriptive Statistics
- Correlation Analysis
- Distribution Analysis
- Outlier Detection
- Time Series Analysis

**Data Visualisation & Business Analysis**
- Matplotlib
- Seaborn
- Data Visualisation
- Performance Analysis
- Business Insight Generation

---

## Author

**Kirby Phillips** | Data Consultant

For any inquiries, contact me: 

Email: kphillips.za@gmail.com

DM: [LinkedIn](https://www.linkedin.com/in/kirbykphillips/)
