#  EdTech Startup – Recorded Lectures Analytics Dashboard

A Power BI analytics solution developed for an EdTech startup looking to expand its recorded lecture offerings. This project transforms raw course data from multiple online learning platforms into actionable business insights that help identify high-performing categories, language preferences, instructor opportunities, and viewer engagement trends.

---

##  Project Objective

The EdTech startup wanted to analyze recorded course offerings across various platforms to:

- Understand the scale and quality of available content.
- Identify popular categories and sub-categories.
- Determine audience language preferences.
- Analyze the impact of subtitles on viewership.
- Discover top-rated instructors for potential collaborations.
- Optimize course duration and accessibility strategies.

The analysis is category-centric and addresses 10 key business questions through interactive Power BI dashboards.

---

#  Repository Structure

```
edtech-recorded-lectures-dashboard/
│
├── README.md
├── EdTech_Recorded_Lectures_Analytics.pbix
│
├── data/
│   └── Online_Courses.csv
│
├── screenshots/
│
├── dax-measures/
│   └── DAX_Measures.txt
|
└── documentation/
    ├── Problem_Statement.pdf
    ├── Data_Model.png
    └── Dashboard_Insights.pdf
```

---

#  Dataset Information

| Attribute | Details |
|------------|----------|
| Source | Online_Courses.csv |
| Total Records | ~8,092 Courses |
| Total Columns | 45 |
| Platforms Covered | Coursera, Udemy, edX, FutureLearn, and others |
| Categories | 18 Unique Categories |

### Key Columns

- Title
- Category
- Sub-Category
- Course Type
- Language
- Subtitle Languages
- Rating
- Number of Viewers
- Duration
- Instructors
- Site

---

#  Data Preparation (Power Query)

The dataset was cleaned and transformed using Power Query:

### ✔ Removed unnecessary columns
Dropped platform-specific attributes and irrelevant metadata.

### ✔ Cleaned Rating column
Removed text values and converted ratings to decimal format.

### ✔ Converted Number of Viewers
Removed commas and changed datatype to Whole Number.

### ✔ Handled missing values
Replaced blanks in:

- Language
- Subtitle Languages
- Course Type

with **"Not Specified"**.

### ✔ Standardized Duration column
Extracted numerical values from text and applied business rules for flexible and monthly courses.

### ✔ Processed Instructor information
Separated multiple instructors to enable individual-level analysis.

### ✔ Created supporting dimension tables

- Category
- Sub-Category
- Language
- Instructor

### ✔ Established relationships
Implemented a Star Schema for efficient modeling and improved report performance.

---

#  Data Model

The report follows a Star Schema approach:

```
                 Category Dimension
                         │
                         │
Language Dimension ─ Fact Online Courses ─ Instructor Dimension
                         │
                         │
                Sub-Category Dimension
```

This structure simplifies calculations and improves dashboard performance.

---

#  Business Questions Answered

| # | Business Question | Visual Used |
|---|------------------|-------------|
| 1 | Total number of courses | KPI Card |
| 2 | Average course rating | KPI Card |
| 3 | Total and average viewers | KPI Cards |
| 4 | Course type distribution by category and sub-category | Stacked Bar Chart |
| 5 | Average views by category, sub-category, and language | Bar Chart |
| 6 | Distribution of languages across courses | Donut Chart |
| 7 | Language preferences by category | Clustered Bar Chart |
| 8 | Subtitle availability versus viewers | Grouped Bar Chart |
| 9 | Top 3 instructors by category and sub-category | Matrix |
| 10 | Course duration versus viewers | Scatter Plot |

---

#  DAX Measures and Functions Used

The dashboard leverages DAX for KPI calculations, context manipulation, filtering, and ranking.

---

## Aggregate Functions

### Total Courses

```DAX
Total Courses =
COUNT('Online Courses'[Title])
```

### Total Viewers

```DAX
Total Viewers =
SUM('Online Courses'[Number of viewers])
```

### Average Rating

```DAX
Average Rating =
AVERAGE('Online Courses'[Rating])
```

### Average Viewers

```DAX
Average Viewers =
AVERAGE('Online Courses'[Number of viewers])
```

---

## CALCULATE()

Used to modify filter context and create custom metrics.

```DAX
Highly Rated Courses =
CALCULATE(
    COUNT('Online Courses'[Title]),
    'Online Courses'[Rating] >= 4.5
)
```

---

## ALL()

Removes filters to calculate overall totals.

```DAX
Viewer Share % =
DIVIDE(
    [Total Viewers],
    CALCULATE(
        [Total Viewers],
        ALL('Online Courses')
    )
)
```

---

## ALLEXCEPT()

Retains category filters while removing others.

```DAX
Category Viewers =
CALCULATE(
    [Total Viewers],
    ALLEXCEPT(
        'Online Courses',
        'Online Courses'[Category]
    )
)
```

---

## FILTER()

Used for conditional calculations.

```DAX
Courses With Subtitles =
CALCULATE(
    COUNT('Online Courses'[Title]),
    FILTER(
        'Online Courses',
        'Online Courses'[Subtitle Languages] <> "Not Specified"
    )
)
```

---

## DIVIDE()

Performs safe division operations.

```DAX
Average Views Per Course =
DIVIDE(
    [Total Viewers],
    [Total Courses]
)
```

---

## RANKX()

Used to identify top instructors.

```DAX
Instructor Rank =
RANKX(
    ALL('Instructor Table'[Instructor]),
    [Average Rating],
    ,
    DESC
)
```

---

#  DAX Concepts Applied

### Aggregation Functions

- SUM()
- COUNT()
- COUNTROWS()
- AVERAGE()

### Context Modification Functions

- CALCULATE()
- ALL()
- ALLEXCEPT()

### Filtering Functions

- FILTER()

### Mathematical Functions

- DIVIDE()

### Ranking Functions

- RANKX()

---

#  Dashboard Features

✔ Interactive KPI Cards

✔ Category and Sub-category Analysis

✔ Language Distribution Analysis

✔ Viewer Engagement Insights

✔ Subtitle Impact Analysis

✔ Instructor Ranking Analysis

✔ Duration versus Views Analysis

✔ Dynamic DAX Measures

✔ Star Schema Data Model

✔ Business-Oriented Insights

---

#  Dashboard Pages

### Overview Page
Provides high-level KPIs including:

- Total Courses
- Average Rating
- Total Viewers
- Average Viewers

### Category Analysis
Analyzes course distribution across categories and sub-categories.

### Language Analysis
Identifies language trends and audience preferences.

### Subtitle Analysis
Evaluates the relationship between subtitle availability and viewership.

### Instructor Analysis
Highlights the top three instructors based on ratings.

### Duration Analysis
Studies the relationship between course duration and viewer engagement.

---

#  Tools and Technologies

| Tool | Purpose |
|--------|---------|
| Power BI Desktop | Dashboard Development |
| Power Query (M) | Data Cleaning and Transformation |
| DAX | Measures and Analytical Calculations |
| Microsoft Excel / CSV | Data Source |
| Data Modeling | Star Schema Implementation |

---

#  How to Use

### Clone the Repository

```bash
git clone https://github.com/your-username/edtech-recorded-lectures-dashboard.git
```

### Open the Power BI Report

Open:

```
EdTech_Recorded_Lectures_Analytics.pbix
```

using Power BI Desktop.

### Refresh Data Source

Navigate to:

```
Home → Transform Data → Data Source Settings
```

Update the CSV file path if necessary and click:

```
Close & Apply
```

---

#  Screenshots

Dashboard screenshots are available inside the **screenshots/** folder.

---

#  Author

**Sanya Khandelwal**

Data Analyst | Power BI Developer

- LinkedIn: https://www.linkedin.com/in/sanya-khandelwal-data-analyst/
- GitHub: https://github.com/sanyakhandelwal
---

# 📜 License

This project is intended for educational and portfolio purposes. The dataset was collected from publicly available EdTech platforms.
