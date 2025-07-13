# MSCS_634_ProjectDeliverable_1

## Deliverable 1: Data Collection, Cleaning, and Exploration

This repository contains the Jupyter Notebook and supporting files for Deliverable 1 of the MSCS 634 project, focusing on the initial phases of data collection, cleaning, and exploratory data analysis (EDA).

### Dataset Selection and Description

**Dataset Name:** `world_bank_data_2025.csv`

**Description:**
This dataset comprises a collection of macroeconomic and social indicators for various countries across multiple years, ranging from 2010 to 2025. Key economic features included are:

* **Inflation (CPI %):** Consumer Price Index inflation.
* **GDP (Current USD):** Gross Domestic Product in current US dollars.
* **GDP per Capita (Current USD):** GDP divided by the mid-year population.
* **Unemployment Rate (%):** Percentage of the labor force that is unemployed.
* **Inflation (GDP Deflator, %):** Inflation based on the GDP deflator.
* **GDP Growth (% Annual):** Annual percentage growth rate of GDP.
* **Current Account Balance (% GDP):** Current account balance as a percentage of GDP.
* **Gross National Income (USD):** Total income earned by a country's people and businesses.

Additionally, the dataset contains `country_name`, `country_id`, and `year` to identify the specific country and time period for each record.

**Justification for Selection:**

This dataset was chosen for the project due to several key reasons that align with the requirements for data collection, cleaning, and exploration:

1.  **Comprehensive Size and Attributes:** Since this project requests the minimal of 8 attributes and 500+ records, the world bank dataset with over 3000+ entries and 16 attributes, the dataset is sufficiently large and diverse to allow for robust analysis and fulfill the project's data size criteria.
2.  **Real-World Relevance:** Sourced from the World Bank, the data represents real-world economic conditions and trends, making the analysis highly relevant and interpretable in a practical context.
3.  **Opportunity for Data Cleaning:** The dataset presented significant data quality challenges, particularly a considerable number of missing values across various economic indicators (e.g., `Public Debt`, `Government Expense`, `Interest Rate`). This provided an excellent opportunity to apply and demonstrate data cleaning techniques, including handling missing data through imputation and identifying columns for removal based on data completeness.
4.  **Scope for Exploratory Data Analysis (EDA):** The variety of economic indicators facilitates extensive EDA, allowing for the exploration of distributions, identification of outliers, and examination of relationships between different economic factors, which are crucial steps before any modeling phase.


# Data Inspection Summary

## 1. Initial Glimpse (`head()`)

- Sample rows illustrate the dataset contains economic indicators per country and year.
- Columns include `Inflation (CPI %)`, `GDP (Current USD)`, `Unemployment Rate (%)`, and `Public Debt (% of GDP)` among others.
- Some values are missing (e.g., `Unemployment Rate` and `Public Debt` show NaNs).
- Data types vary: numeric columns are mostly `float64`, with some integers and objects (`string`).

## 2. Structure Overview (`info()`)

- Dataset has **3472 rows** and **16 columns**.
- Data types:
  - 2 categorical (`object`)
  - 1 integer (`int64`)
  - 13 numeric (`float64`)
- Missing values present in several key columns:
  - `Inflation (CPI %)`: 2694 non-null (~22% missing)
  - `Interest Rate (Real, %)`: 1735 non-null (~50% missing)
  - `Public Debt (% of GDP)`: 852 non-null (~75% missing)
  - Memory usage approximately **434 KB**.

## 3. Statistical Summary (`describe()`)

- Numeric columns vary widely in scale:
  - GDP ranges from ~32 million USD to over 27 trillion USD.
  - Inflation (CPI %) mean ≈ 6.23% with large std dev (~19.7%), indicating high volatility and outliers (max ~557%).
  - Median GDP per Capita ≈ 6,827 USD, but max exceeds 256,000 USD, showing large disparities.
  - Public Debt mean ≈ 61.9% of GDP, but data is sparse with high std deviation (~40.4%).
  - Some columns show extreme minimum and maximum values, suggesting outliers or anomalies.


### Major Steps Taken in Data Cleaning and Exploration

#### Data Cleaning:

## Step 1: Check Missing Values
- Inspect the dataset to identify columns with missing data.
- Use `df.isnull().sum()` to count missing values per column.

## Step 2: Fill Missing Numeric Values with Median
- Numeric missing values are filled with the median of each column.
- Median is chosen because it is less sensitive to outliers than the mean.

## Step 3: Fill Missing Categorical Values with Mode
- Missing categorical data is filled with the most frequent category (mode).
- Ensures no null categories remain for categorical features.

## Step 4: Drop Columns with Excessive Missing Data (Optional)
- Columns with more than 50% missing values are dropped to maintain data quality.
- This threshold can be adjusted based on project needs.

## Step 5: Remove Exact Duplicate Rows
- Remove any duplicated rows to avoid redundancy and bias.
- Use `df.drop_duplicates()`.

## Step 6: Check and Handle Duplicates on Key Identifiers
- Identify duplicates based on important keys like `country_name` and `year`.
- These should ideally be unique; investigate and resolve duplicates if found.

## Step 7: Standardize Categorical Data
- Clean categorical fields such as `country_name` by trimming whitespace.
- Inspect unique values to identify inconsistent naming.
- Replace inconsistent names manually if necessary.

## Step 8: Detect and Handle Outliers
- Use statistical methods (e.g., z-score) to identify outliers in numeric columns.
- Cap extreme values at the 1st and 99th percentiles to reduce their impact.

## Step 9: Ensure Correct Data Types
- Convert columns to appropriate data types:
  - `year` as integer.
  - Categorical columns to `category` dtype for memory efficiency.

#### Exploratory Data Analysis (EDA):

## 1. Distributions of Key Variables  
Visualized the distributions of important economic indicators such as Inflation (CPI %), GDP (Current USD), and GDP per Capita (Current USD) using histograms. This helped understand the spread and skewness of these variables across the dataset.

## 2. Outlier Detection  
Used boxplots to identify outliers in variables like Inflation (CPI %) and GDP per Capita (Current USD). Boxplots highlighted extreme values that could potentially influence the analysis.

## 3. Feature Relationships  
- Calculated the correlation matrix among numeric variables and visualized it with a heatmap to identify strong and weak relationships between features.  
- Created scatter plots (e.g., GDP vs GDP per Capita) to visually examine the relationships between key pairs of variables.

## 4. Time Series Analysis  
Plotted GDP Growth (% Annual) over time for selected countries (United States, China, India, Brazil) to analyze economic growth trends and patterns across years.


### Key Insights from Analysis

## 1. Data Distributions and Outliers

### Inflation (CPI %)
- Distribution is heavily right-skewed; most observations fall in lower bins (1332 in [-1.57, 3.17], 1674 in [3.17, 7.92]).
- Mean (4.76%) is higher than median (3.21%), confirming right-skewness.
- Significant number of outliers (314 out of 3472) with values up to 45.91%, indicating a few countries face very high inflation.

### GDP (Current USD) and GDP per Capita (Current USD)
- Both show extreme right-skewness; majority concentrated in lowest bins (3133/3472 GDP in [1.87×10⁸, 5.03×10¹¹] USD, 2352 GDP per Capita in [442.07, 12149.87] USD).
- Large difference between mean and median GDP per Capita (mean: 1.61×10⁴, median: 6.82×10³).
- Presence of outliers (39 for GDP, 417 for GDP per Capita) reaching very high values (up to 5.03×10¹² for GDP, 1.17×10⁵ for GDP per Capita), reflecting global economic disparities.

## 2. Feature Relationships (Correlation Matrix)

### Strong Positive Correlations
- GDP (Current USD) and Gross National Income (USD): very strong correlation (0.999561).
- Government Expense (% GDP) and Government Revenue (% GDP): strong correlation (0.830207).
- Tax Revenue (% GDP) correlated strongly with Government Revenue (0.715597) and Government Expense (0.627816).

### Moderate Positive Correlations
- Inflation (CPI %) and Inflation (GDP Deflator, %): moderately correlated (0.639839).
- GDP per Capita (Current USD) moderately correlates with Current Account Balance (% GDP) (0.305481), Government Revenue (% GDP) (0.273241), and Tax Revenue (% GDP) (0.216269).

### Weak or Negligible Correlations
- GDP Growth (% Annual) weakly correlated with most variables, including Inflation (CPI %) (−0.050840).
- Unemployment Rate (%) weakly correlated with GDP per Capita (−0.122536), showing higher GDP per capita does not strongly imply lower unemployment linearly.

## 3. GDP Growth Trends in Key Economies (2010-2025)

### United States
- Stable positive growth (1.5%-3.0%) from 2010-2019.
- Contraction in 2020 (−2.16%) due to global events.
- Strong rebound in 2021 (6.05%) and continued growth through 2025.

### China
- High growth (>6%) from 2010 to 2018.
- Growth decelerated after 2019, sharp drop in 2020 (2.23%).
- Strong rebound in 2021 (8.44%), then moderate growth thereafter.

### India
- Strong growth >7% from 2010-2018.
- Severe contraction in 2020 (−5.77%).
- Very strong recovery in 2021 (9.68%), continued robust growth through 2023.

### Brazil
- Strong growth in 2010 (7.52%).
- Economic recessions in 2015 (−3.54%) and 2016 (−3.27%).
- Contraction in 2020 (−3.27%), recovery in 2021 (4.76%) with volatile, lower growth afterwards.

Overall, these insights highlight diverse economic conditions and resilience across countries, the importance of robust statistical approaches due to skewed distributions and outliers, and the varied impact of global shocks especially around 2020.

### Challenges Encountered and Addressed

1. **Learning Python and Pandas**  
   - Used to Java, so Python’s syntax and data handling with Pandas was new and different.  
   - Solved by reading tutorials and practicing simple examples and since the basics are same it was easy to make sense.

2. **Getting Used to Jupyter Lab**  
   - Figuring out how to run code in cells and use markdown took some time.  
   - Solved by exploring Jupyter step-by-step and keeping code in small parts.
   
3. **Debugging Errors**  
   - Errors in Jupyter were sometimes hard to understand compared to Java.  
   - Solved by running code step-by-step and searching for solutions online.

---