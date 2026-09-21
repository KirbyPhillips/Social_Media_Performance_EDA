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

This project analyses 5,600 social media posts across 6 platforms to understand which factors are associated with stronger engagement and where businesses should focus their content strategy.

The analysis investigates:

- Which content factors drive the highest engagement rate?
- Does posting day or time meaningfully affect engagement?
- Which metrics provide the most useful measures of content performance?

The project addresses these questions through exploratory data analysis, statistical analysis, data visualisation, and business-focused interpretation.

---

## 2. Data Inspection

### What this code block is doing and why it's needed

- Checks the dataset’s **size, structure, data types, and missing values** before analysis.
- `df.shape` shows the number of rows and columns.
- `df.info()` shows column names, data types, and non-null counts.
- `audit()` provides a deeper column-level check of **nulls, unique values, and sample values**.
- Memory usage confirms the dataset is lightweight enough to work with efficiently.

### Interpretation of the output

- The dataset contains **5,600 posts across 24 columns**.
- Only **Clicks** and **Click_Through_Rate** contain missing values, with **3,740 missing values (66.8%)** each.
- The missing click data appears **structural rather than random**, likely relating to specific platforms and post types that do not generate trackable clicks.
- Data types are clean and logical: strings are stored as objects, counts as integers, rates as floats, and dates as datetime.
- The dataset is **lightweight at just over 1MB**, so it can be processed efficiently throughout the analysis.

---

## 3. EDA Health Check

### What this code block is doing and why it's needed

- Runs a **complete EDA health check** covering dataset size, memory, missing values, duplicates, numeric statistics, categorical summaries, and skewness.
- Consolidates the checks into one structured overview to provide **situational awareness before deeper analysis**.

### Interpretation of the output

- The dataset has **5,600 rows and 24 columns**, with **no duplicates** and only **Clicks** and **Click_Through_Rate** missing (66.79% each).
- Engagement and other performance metrics are **strongly right-skewed**, with a small number of high-performing posts pulling the mean above the median.
- The dataset contains **6 platforms, 5 content categories, 7 post types, and 8 regions**; Video is the dominant post type and Educational is the largest content category.
- **Nine numeric columns are highly skewed**, so median performance is more representative than the mean and high-performing outliers should be investigated rather than removed.
- The **66.79% missing rate for click data** limits click-based analysis to **1,860 posts** and should be treated as a key data limitation.

---

## 4. Descriptive Statistics

### What this code block is doing and why it's needed

- Calculates summary statistics for the dataset, including **mean, median, skewness, kurtosis, and null counts** across key metrics.
- Separates columns into **numeric, categorical, and datetime** types to understand the structure of the data.
- Produces a **correlation matrix** to identify relationships between numeric variables and guide dashboard metrics.

### Interpretation of the output

- The dataset contains **14 numeric, 8 categorical, and 2 datetime columns**, supporting both performance and time-based analysis.
- **Engagement, Impressions, Views, Likes, Shares, and Comments are strongly right-skewed**, making the median more representative of typical performance than the mean.
- **Engagement_Rate and Click_Through_Rate are more consistent**, with mean and median both around 0.15 and 0.02 respectively; however, CTR is limited by **3,740 missing values**.
- **Engagement strongly correlates with Views and Impressions (0.90)**, while Likes, Shares, Comments, and Clicks also move closely together. In contrast, **Engagement_Rate negatively correlates with reach metrics**, showing that high reach does not necessarily mean high engagement rate.
- The correlation results support keeping **total Engagement and Engagement_Rate as separate dashboard metrics**, while Post_Hour and geographic coordinates show little relationship with performance.

---

## 5. Missing Values

### What this code block is doing and why it's needed

- Measures and visualises missing data by calculating **null counts and percentages** for each column.
- Identifies columns exceeding a **30% missing-value threshold** and investigates missing Clicks and Click_Through_Rate by **Platform and Post_Type**.
- Uses a **heatmap and bar chart** to understand the pattern and concentration of missing data.

### Interpretation of the output

- Only **Clicks** and **Click_Through_Rate** contain missing values, with **3,740 missing values each**; all other columns are complete.
- The heatmap shows the missing values form a **concentrated pattern rather than random gaps**, indicating structural missingness.
- Missing Clicks are concentrated across **YouTube (1,320), X.com (1,201), Instagram (998), and LinkedIn (221)**, while TikTok and Facebook have no missing click data.
- Missing Clicks are concentrated mainly in **Video (1,871), Image (922), Live Stream (538), and Text (371)** posts, confirming the relationship with post type.
- The missing values should **not be filled with zeros or used to remove rows**; click-based analysis should be limited to the **1,860 posts with valid click data**.

---

## 6. Univariate Analysis

### What this code block is doing and why it's needed

- Examines each variable individually using **histograms, boxplots, QQ plots, and categorical bar charts**.
- Assesses the **distribution, spread, outliers, and category frequencies** before analysing relationships between variables.

### Interpretation of the output

- **Engagement, Impressions, Views, Likes, Shares, Comments, and Clicks are strongly right-skewed**, with high-performing posts creating extreme upper-end values; **Engagement_Rate is the most stable metric**.
- **YouTube, TikTok, and X.com account for 67.5% of posts**, while Video represents **52.6% of all post types**, making these distributions important when comparing performance.
- **Educational content is the largest category (36.2%)**, while the eight regions have relatively even representation, supporting reliable regional comparisons.
- **Medium engagement dominates (55.5%)**, and the top three hashtags — **#SuccessStory, #CustomerStory, and #ProductDemo** — account for **61.6% of all posts**.
- The consistent skew in volume metrics reinforces using **median rather than mean** for performance benchmarks, while **Engagement_Rate** should be used as the primary comparison metric across platforms, post types, and content categories.

---

## 7. Bivariate Analysis

### What this code block is doing and why it's needed

- Examines relationships between pairs of variables using **boxplots, violin plots, group-level statistics, and cross-tabulations**.
- Compares Engagement_Rate across **Platform, Post_Type, and Content_Category**, and examines the relationship between Post_Type and Engagement_Level.
- Identifies which content dimensions are associated with meaningful differences in performance.

### Interpretation of the output

- **Engagement_Rate is consistent across platforms**, ranging from a median of **0.14 on LinkedIn to 0.16 on Instagram**, indicating that platform alone is not a meaningful differentiator.
- **Post_Type shows more variation**: Image and PDF have the highest median engagement rate at **0.18**, while Video is lowest at **0.14**, despite accounting for **52.6% of all posts**.
- **Content_Category shows the strongest variation**: Customer Story and Educational both have a median engagement rate of **0.20**, compared with **0.08 for Entertainment**.
- **Video has the lowest proportion of High engagement (22%) and highest proportion of Low engagement (29%)**, while Image and PDF posts have higher proportions of High engagement.
- The findings indicate that **content category and post type are stronger performance differentiators than platform**, with Educational and Customer Story content consistently performing strongly.

---

## 8. Correlation & Heatmap

### What this code block is doing and why it's needed

- Calculates the **correlation between every pair of numeric metrics** and visualises the results using a colour-coded heatmap.
- Masks the upper triangle to remove duplicate values, leaving each unique correlation pair shown once.
- Extracts and ranks the **top 15 most correlated pairs** by absolute correlation strength.
- Uses correlation analysis to identify which metrics **move together and which measure different behaviour**.
- Supports dashboard design by identifying metrics that are redundant versus those that provide distinct performance insights.

### Interpretation of the output

- **Engagement, Impressions, Likes, Shares, Comments, Views, and Clicks form a strong correlation cluster**, with correlations ranging from **0.80 to 1.00**, showing that these volume metrics generally move together.
- **Engagement_Rate behaves differently**, showing negative correlations with reach and interaction metrics, including **-0.32 with Impressions and Views**, and a near-zero correlation of **0.04 with Engagement**.
- **Impressions and Views have a perfect correlation of 1.00**, indicating they are essentially measuring the same metric in this dataset; including both in the dashboard would therefore be redundant.
- **Likes, Shares, Comments, and Clicks correlate strongly with Impressions and Views (0.88–0.95)**, while Engagement correlates **0.87 with both Views and Impressions**, confirming its relationship with overall interaction volume.
- For dashboard design, **Impressions can represent reach instead of Views**, while **Engagement and Engagement_Rate should be reported separately** because total Engagement reflects raw interaction volume, whereas Engagement_Rate reflects the proportion of the audience that interacted.

---

## 9. Outlier Detection

### What this code block is doing and why it's needed

- Identifies unusually high or low values using **IQR and Z-score methods**.
- IQR is more suitable for **skewed data**, while Z-score identifies more extreme deviations from the mean.
- Flags outliers with an **Is_Outlier** column rather than removing them, allowing them to be included or excluded during analysis.

### Interpretation of the output

- **Engagement has the highest IQR outlier count**, with 435 posts (7.8%), followed by Comments (7.1%), Likes (5.5%), and Shares (5.2%).
- **Engagement_Rate has zero IQR outliers**, reinforcing that it is a stable and consistent performance metric.
- The Z-score method flags **263 rows**, fewer than IQR because it is more conservative with the dataset’s strongly skewed volume metrics.
- Boxplots confirm that Engagement, Likes, Shares, and Comments contain clusters of high-performing outliers, while Impressions and Views have fewer but more extreme outliers.
- The outliers represent **genuinely high-performing posts rather than data errors**, so they should be retained and flagged rather than removed; an optional dashboard filter can allow analysis with or without extreme posts.

---

## 10. Time Series EDA

### What this code block is doing and why it's needed

- Analyses performance patterns across **monthly, weekly, and hourly time dimensions**.
- Converts `Post_Date` into a proper datetime format and groups **total engagement by month** to identify overall trends.
- Extracts the **day of week** from each post date to compare average engagement rates.
- Groups posts by **posting hour** to identify which times of day perform best.
- Provides data-driven insight into **when to post**, a key requirement of social media performance analysis.

### Interpretation of the output

- **Monthly total engagement remains relatively stable** from January 2024 through approximately April 2025, generally ranging between **35 million and 45 million per month**, with a dip in April 2024 followed by recovery.
- The sharp decline in **May 2025 is a data collection artefact**, likely caused by the dataset containing only part of the month; this period should therefore be excluded or flagged when reporting trends.
- **Average engagement rate is almost identical across all seven days**, with values around **0.15** and differences of less than 0.01, indicating that day of week does not meaningfully predict performance.
- **Posting hour shows similarly limited variation**, with average engagement rates between **0.148 and 0.158**; the highest-performing hours are 13:00, 16:00, 14:00, 18:00, and 17:00, all at approximately **0.16**.
- Overall, **content appears to matter more than timing** in this dataset: content category and post type produce engagement-rate differences of **0.06–0.12**, compared with less than **0.01** across days and posting hours.

---

## 11. Categorical Deep Dive

### What this code block is doing and why it's needed

- Examines each **categorical variable** by checking unique values, the most common category, percentage share, and potential imbalance.
- Flags categories where a single value represents more than **90% of rows** to assess whether segmentation is meaningful.
- Calculates average **Engagement_Rate** for each value within Platform, Post_Type, and Content_Category.
- Consolidates the key categorical findings into a **ranked summary** of engagement performance.
- Identifies which **platform, post type, and content category** are most associated with higher engagement rates.

### Interpretation of the output

- No categorical variable is highly imbalanced, with no single value exceeding the **90% threshold**; Video is the most common post type at **52.6%** but remains suitable for comparison.
- **Instagram leads platform performance at 0.16**, while LinkedIn is lowest at 0.14; the **0.02 range** makes platform the weakest differentiator.
- **Image and PDF posts lead at 0.18**, while Video is lowest at 0.14 despite representing **52.6% of all posts**; the **0.04 range** is twice that of platforms.
- **Content Category shows the strongest variation**, with Educational and Customer Story at **0.20** and Entertainment at **0.08**, producing a **0.12 range**.
- The findings indicate that **content category is the strongest differentiator, followed by post type and then platform**, making category-level performance an important focus for the dashboard and portfolio analysis.

---

## 12. Key Findings & Business Impact

### Key Findings

1. **Content strategy has a stronger relationship with engagement than platform or timing.** Educational and Customer Story content achieve the highest Engagement_Rate at **0.20**, compared with **0.08 for Entertainment**. Post Type shows a smaller difference, while Platform has the narrowest range.

2. **Posting time does not meaningfully differentiate engagement.** Average Engagement_Rate is approximately **0.15 across all days**, while posting hours range only from **0.148 to 0.158**. The differences are negligible compared with those observed across content categories and post types.

3. **Engagement volume and Engagement_Rate provide different performance insights.** Engagement is strongly related to reach and interaction volume, while Engagement_Rate measures a different aspect of performance. Impressions and Views are perfectly correlated at **1.00**, making them redundant as separate dashboard metrics.

---

### Business Impact & Financial Value

1. **Content strategy:**
   - The **0.12 Engagement_Rate gap** between the highest and lowest content categories provides a measurable basis for evaluating content strategy.
   - Financial value could be assessed through **engagement, clicks, leads, conversions, CAC, and revenue**.

3. **Content scheduling:**
   - The limited variation in Engagement_Rate across days and hours suggests minimal performance differences from timing.
   - Financial value could be assessed through **content costs, cost per engagement, and campaign ROI**.

5. **Performance measurement:**
   - Separating Engagement from Engagement_Rate and removing redundant metrics creates a clearer performance framework.
   - Financial value could be assessed through **CTR, leads, conversions, revenue, ROAS, and marketing ROI**.
---

## 13. In Hindsight

This analysis demonstrated that social media performance could be analysed to identify meaningful differences in engagement across content, post types, platforms, and timing. Looking back, there are several areas I would expand in a future iteration.

### Data & Analysis

- Incorporate additional business data such as **clicks, conversions, leads, and revenue** to connect engagement with financial outcomes.
- Investigate whether the strongest content categories remain consistent across different platforms and audience segments.

### Dashboard & Business Application

- Develop a more interactive dashboard focused on **content performance, engagement, and business KPIs**.
- Test whether insights from the analysis translate into measurable improvements in **engagement, conversions, and marketing ROI**.

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
