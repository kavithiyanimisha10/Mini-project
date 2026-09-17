# 📱 Amazon Mobile Phones Web Scraping & Data Analysis

## 📌 Project Overview

This project demonstrates an end-to-end **web scraping and data analysis workflow** using Python.

The project collects mobile phone product information from Amazon using **Selenium**, stores the collected data in an Excel file, and then performs **data cleaning, filtering, statistical analysis, grouping, visualization, correlation analysis, and outlier treatment** using Python.

The main objective is to transform raw e-commerce product data into a structured dataset and extract useful patterns and insights from it.

---

## 🎯 Project Objectives

The main objectives of this project are:

* Scrape mobile phone product information from Amazon.
* Collect important product attributes such as price, discount, rating, brand and reviews.
* Store the scraped data in a structured Excel dataset.
* Clean and preprocess the collected data.
* Handle missing and duplicate values.
* Convert relevant columns into appropriate data types.
* Filter products based on analytical conditions.
* Perform descriptive statistical analysis.
* Analyze products by brand.
* Compare average prices, maximum discounts and ratings across brands.
* Create different types of visualizations.
* Analyze relationships between numerical variables.
* Detect and handle outliers using the IQR method.
* Generate business-oriented insights from the dataset.

---

## 🛠️ Technologies & Libraries Used

### Programming Language

* Python

### Web Scraping

* Selenium

### Data Manipulation & Analysis

* Pandas
* NumPy

### Data Visualization

* Matplotlib
* Seaborn

### Data Storage

* Excel (`.xlsx`)

### Development Environment

* Jupyter Notebook
* Google Chrome
* Chrome WebDriver

---

## 📊 Data Collected

The web scraping process collects the following information:

| Column       | Description                    |
| ------------ | ------------------------------ |
| Product Name | Name/title of the mobile phone |
| Price        | Listed product price           |
| Discount     | Discount percentage            |
| Rating       | Customer rating out of 5       |
| Brand        | Product brand                  |
| Review       | Number of customer reviews     |
| Deal         | Deal information               |
| Delivery     | Delivery information           |
| URL          | Product page URL               |

The notebook creates a Pandas DataFrame containing these fields after scraping the product listings.

---

# 🔄 Project Workflow

```text
Amazon Website
      ↓
Selenium Web Scraping
      ↓
Product Data Collection
      ↓
Excel Dataset
      ↓
Data Loading with Pandas
      ↓
Data Cleaning
      ↓
Data Filtering
      ↓
Statistical Analysis
      ↓
Group-by Analysis
      ↓
Data Visualization
      ↓
Correlation Analysis
      ↓
Outlier Detection & Treatment
      ↓
Business Insights
```

---

# 1️⃣ Web Scraping

Selenium is used to open the browser, navigate to Amazon, search for **mobile phones**, and identify product listing elements.

The project uses XPath selectors to extract product information.

### Example fields extracted:

* Product name
* Price
* Discount
* Rating
* Brand
* Reviews
* Deals
* Delivery information
* Product URL

Exception handling is used while extracting individual fields so that missing information does not stop the complete scraping process.

The collected information is converted into a Pandas DataFrame.

---

# 2️⃣ Dataset Creation

After collecting the product information, the data is organized into a Pandas DataFrame with the following columns:

```python
[
    "Product Name",
    "Price",
    "Discount",
    "Rating",
    "Brand",
    "Review",
    "Deal",
    "Delivery",
    "URL"
]
```

The notebook also includes an option to save the scraped data as an Excel file:

```python
df.to_excel("Amazon_products.xlsx", index=True)
```

---

# 3️⃣ Loading the Dataset

The collected dataset is loaded from an Excel file using Pandas:

```python
df = pd.read_excel("Amazon_products.xlsx")
```

This allows the scraped data to be processed and analyzed separately from the web-scraping stage.

---

# 4️⃣ Data Cleaning

Data cleaning is performed before analysis.

## Data Type Conversion

The project converts fields such as:

* Discount
* Rating
* Review

into numerical values.

For example, the discount percentage is extracted using a regular expression:

```python
df.Discount = df.Discount.astype(str).str.extract(
    r"(\d+(?:\.\d+)?)%"
)[0]
```

The rating text is also cleaned by removing:

```text
out of 5 stars
```

and then converting the column to a numeric data type.

---

## Missing Value Handling

The project handles missing values in several columns.

Examples include:

```python
df.Discount = df.Discount.fillna(0)

df.Rating = df.Rating.fillna(df.Rating.median())

df.Review = df.Review.fillna(df.Review.median())

df.Deal = df.Deal.fillna("No deal")

df.Delivery = df.Delivery.fillna("Not available")

df.URL = df.URL.fillna("Not available")
```

This helps prepare the dataset for further analysis.

---

# 5️⃣ Data Filtering

The project performs conditional filtering to identify products satisfying specific criteria.

For example:

```python
df[(df.Discount > 30) & (df.Rating > 2.5)]
```

This identifies products with:

* More than 30% discount
* Rating greater than 2.5

This type of filtering can help identify products that meet selected business or customer criteria.

---

# 6️⃣ Statistical Analysis

Descriptive statistics are generated using:

```python
df.describe().T
```

This provides statistical information about numerical columns, helping understand:

* Count
* Mean
* Standard deviation
* Minimum
* Maximum
* Quartiles

The project also counts products by brand:

```python
df.value_counts("Brand")
```

---

# 7️⃣ Brand-Level Analysis

Group-by analysis is used to compare products across brands.

## Average Price by Brand

```python
df[["Brand", "Price"]].groupby("Brand").mean().reset_index()
```

This calculates the average product price for each brand.

## Maximum Discount by Brand

```python
df[["Brand", "Discount"]].groupby("Brand").max().reset_index()
```

This identifies the highest discount available for each brand.

## Highest Rating by Brand

```python
df[["Brand", "Rating"]].groupby("Brand").max().reset_index()
```

This compares the highest observed rating for each brand.

These analyses allow product characteristics to be compared at the brand level.

---

# 8️⃣ Data Visualization

Several visualization techniques are used to understand the dataset.

## 📈 Histogram

Histograms are used to examine the distribution of numerical variables such as rating and other continuous columns.

```python
df.Rating.plot(kind="hist")
```

---

## 🥧 Pie Chart

A pie chart is created to visualize brand distribution:

```python
df["Brand"].value_counts().plot(
    kind="pie",
    autopct="%1.1f%%"
)
```

This helps understand the proportion of products belonging to different brands.

---

## 📊 Bar Chart

A bar chart is used to compare the number of products by brand.

```python
df.Brand.value_counts().plot(kind="bar")
```

---

## 🔵 Scatter Plot

A scatter plot is created to examine the relationship between:

* Price
* Discount

```python
sns.scatterplot(
    data=df,
    x="Price",
    y="Discount"
)
```

---

## 🔗 Pair Plot

A pair plot is used to visualize relationships among multiple continuous variables.

```python
sns.pairplot(cont_df)
```

---

# 9️⃣ Correlation Analysis

The project performs correlation analysis on:

* Price
* Discount
* Rating
* Review

```python
cont_df = df[["Price", "Discount", "Rating", "Review"]]

cor_df = cont_df.corr()
```

A heatmap is then created to visually represent the correlation matrix:

```python
sns.heatmap(
    data=cor_df,
    annot=True
)
```

This helps identify the strength and direction of relationships between numerical variables.

---

# 🔟 Outlier Detection

The project uses the **Interquartile Range (IQR)** method to identify and handle outliers.

The following steps are performed:

1. Calculate Q1.
2. Calculate Q3.
3. Calculate IQR.
4. Calculate lower and upper limits.
5. Apply clipping to restrict extreme values.

The formula used is:

```text
IQR = Q3 - Q1

Lower Limit = Q1 - 1.5 × IQR

Upper Limit = Q3 + 1.5 × IQR
```

The notebook applies this approach to:

* Price
* Discount
* Rating

and visualizes the distributions using box plots.

---

# 1️⃣1️⃣ Brand vs Maximum Price Analysis

The project also groups products by brand and calculates the maximum price for each brand:

```python
df[
    ["Brand", "Price"]
].groupby(
    "Brand"
).max().reset_index()
```

The result is visualized using a bar chart to compare maximum observed product prices across brands.

---

# 📈 Key Analytical Areas

The project focuses on answering questions such as:

* Which brands appear most frequently in the scraped dataset?
* What is the average product price for each brand?
* What is the maximum discount offered by each brand?
* What are the highest observed ratings for different brands?
* How are product ratings distributed?
* How are products distributed across brands?
* Is there a visible relationship between price and discount?
* How are price, discount, rating and reviews correlated?
* Which numerical variables contain potential outliers?
* How do maximum product prices vary between brands?

---

# 💡 Skills Demonstrated

This project demonstrates practical knowledge of:

### Python

* Variables and data structures
* Loops
* Exception handling
* String manipulation
* Regular expressions
* Data processing

### Selenium

* Browser automation
* Finding web elements
* XPath selectors
* Extracting web page data

### Pandas

* DataFrames
* Data loading
* Data cleaning
* Missing-value handling
* Filtering
* Sorting
* GroupBy
* Descriptive statistics

### Data Visualization

* Histograms
* Bar charts
* Pie charts
* Scatter plots
* Pair plots
* Box plots
* Heatmaps

### Data Analysis

* Descriptive statistics
* Group-based analysis
* Correlation analysis
* Outlier detection
* IQR method

---

# 📁 Project Structure

```text
Amazon-Mobile-Phones-Data-Analysis/
│
├── MINI_project_nimisha.ipynb
├── Amazon_products.xlsx
└── README.md
```

### Files

**`MINI_project_nimisha.ipynb`**
Contains the complete Python code, scraping process, data cleaning, analysis and visualizations.

**`Amazon_products.xlsx`**
Contains the scraped product dataset used for analysis.

**`README.md`**
Provides documentation and explanation of the project.

---

# ⚠️ Important Note

The product information collected through web scraping represents the data available at the time the notebook was executed. Product prices, discounts, ratings, availability and other information can change over time.

The project is intended for **educational and portfolio purposes** to demonstrate web scraping and data analysis techniques.

---

# 🚀 Future Improvements

The project can be extended by:

* Scraping multiple Amazon search pages.
* Collecting a larger dataset.
* Improving missing-value handling.
* Cleaning product names and brand names more systematically.
* Converting price values into proper numerical format.
* Adding additional product attributes.
* Creating an interactive dashboard using Power BI.
* Performing time-based price analysis.
* Building a product recommendation or price-comparison analysis.
* Automating the data collection and analysis pipeline.

---

# 👩‍💻 Author

**Nimisha Kavithiya**

B.Com Graduate |  Data Analyst

### Skills

Python • SQL • MySQL • Excel • Power BI • Pandas • NumPy • Matplotlib • Seaborn

---

## ⭐ Project Highlights

> **Web Scraping → Data Cleaning → Data Analysis → Visualization → Insights**

This project demonstrates an end-to-end approach to working with real-world e-commerce data using Python.
