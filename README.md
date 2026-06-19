# EdTech Startup — Recorded Lectures Analytics Dashboard

> A Power BI analytics solution for an EdTech startup looking to expand its recorded lecture offerings. This project transforms raw course data from multiple platforms into a category-wise strategic intelligence dashboard.

---

## Project Overview

An EdTech startup collected course data from various online learning platforms and required a structured analytical approach to:

- Understand the scale and quality of available course content.
- Identify high-performing categories, sub-categories, and languages.
- Discover instructor talent worth approaching for content creation.
- Determine optimal course duration and subtitle strategies.

The analysis is category-centric and answers 10 business questions through interactive dashboards and DAX-driven KPIs.

---

## Dataset

| Attribute | Details |
|------------|---------|
| Source File | Online_Courses.csv |
| Total Records | ~8,092 courses |
| Platforms Covered | Coursera, Udemy, edX, FutureLearn, and others |
| Total Columns | 45 |
| Categories | 18 unique categories |

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

## Data Preparation

The following transformations were performed using Power Query:

1. Removed irrelevant columns.
2. Cleaned Rating column and converted to decimal.
3. Converted Number of Viewers into numeric format.
4. Replaced null values with "Not Specified".
5. Parsed and standardized Duration values.
6. Split multiple instructors for instructor-level analysis.
7. Created supporting dimension tables.
8. Built a star schema data model with relationships.

---

## Dashboard Insights

The dashboard answers the following business questions:

| Question | Visual |
|------------|--------|
| Total number of courses | KPI Card |
| Average course rating | KPI Card |
| Total and average viewers | KPI Cards |
| Course type distribution by category and sub-category | Stacked Bar Chart |
| Average views by category, sub-category, and language | Bar Chart |
| Language distribution across courses | Donut Chart |
| Language preference by category | Clustered Bar Chart |
| Subtitle availability vs views | Grouped Bar Chart |
| Top 3 instructors by category and sub-category | Matrix |
| Course duration vs views | Scatter Plot |

---

# DAX Measures Used

The dashboard utilizes several DAX functions to create KPIs and perform advanced calculations.

## Aggregate Functions

### Total Courses

```DAX
Total Courses =
COUNT('Online Courses'[Title])
```

### Average Rating

```DAX
Average Rating =
AVERAGE('Online Courses'[Rating])
```

### Total Viewers

```DAX
Total Viewers =
SUM('Online Courses'[Number of viewers])
```

### Average Viewers

```DAX
Average Viewers =
AVERAGE('Online Courses'[Number of viewers])
```

---

## CALCULATE Function

Used to modify filter context and create custom calculations.

### Courses Above 4.5 Rating

```DAX
Highly Rated Courses =
CALCULATE(
    COUNT('Online Courses'[Title]),
    'Online Courses'[Rating] >= 4.5
)
```

---

## ALL Function

Removes filters and calculates overall values.

### Percentage of Total Viewers

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

## ALLEXCEPT Function

Preserves Category filters while removing others.

### Category-wise Total Viewers

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

This allows comparison of sub-categories within each category.

---

## RANKX Function

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

This measure is used to identify the Top 3 instructors for each category and sub-category.

---

## DIVIDE Function

Used for safe division and percentage calculations.

```DAX
Average Views Per Course =
DIVIDE(
    [Total Viewers],
    [Total Courses]
)
```

---

## FILTER Function

Used inside CALCULATE for conditional calculations.

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

## Data Model

A Star Schema was implemented:

- Fact Table: Online Courses
- Dimension Tables:
  - Category
  - Sub-Category
  - Language
  - Instructor

This structure improves performance and simplifies DAX calculations.

---

## Tools Used

| Tool | Purpose |
|--------|---------|
| Power BI Desktop | Dashboard development |
| Power Query (M) | Data cleaning and transformation |
| DAX | Measures, KPIs and analytical calculations |
| Microsoft Excel / CSV | Data source |

---

## Key DAX Functions Applied

- SUM()
- AVERAGE()
- COUNT()
- COUNTROWS()
- CALCULATE()
- ALL()
- ALLEXCEPT()
- FILTER()
- DIVIDE()
- RANKX()

---

## Author

**Sanya Khandelwal**

Data Analyst | Power BI Developer

LinkedIn: https://www.linkedin.com/in/sanya-khandelwal-data-analyst/

GitHub: https://github.com/sanyakhandelwal

---

## License

This project is intended for portfolio and educational purposes. The dataset was collected from publicly available EdTech platforms.
