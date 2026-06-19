## Repository Structure

```text
edtech-recorded-lectures-dashboard/
│
├── README.md
├── EdTech_Recorded_Lectures_Analytics.pbix
│
├── data/
│   └── Online_Courses.csv
│
├── dashboard_screenshots/
│
└── documentation/
    └── Problem_Statement.pdf
```

## Project Overview

This project presents a Power BI analytics solution developed for an EdTech startup aiming to expand its recorded lecture offerings. The dashboard transforms raw course data into meaningful insights that support strategic decision-making.

## Dataset Information

| Attribute | Details |
|------------|----------|
| Source File | Online_Courses.csv |
| Total Records | ~8,092 Courses |
| Total Columns | 45 |
| Platforms Covered | Coursera, Udemy, edX, FutureLearn, and others |
| Categories | 18 Unique Categories |

## Data Preparation

The following transformations were performed using Power Query:

- Removed irrelevant columns and metadata.
- Cleaned and standardized the Rating column.
- Converted Number of Viewers to numeric format.
- Handled missing values in Language, Subtitle Languages, and Course Type.
- Standardized Duration values according to business rules.
- Processed Instructor information for individual-level analysis.
- Created supporting dimension tables.
- Implemented relationships using a star schema.

## Data Model

The report follows a star schema approach consisting of:

- Fact Table:
  - Online Courses

- Dimension Tables:
  - Category
  - Sub-Category
  - Language
  - Instructor

## Business Questions Addressed

1. Total number of courses.
2. Average course rating.
3. Total and average number of viewers.
4. Distribution of course types by category and sub-category.
5. Average views by category, sub-category, and language.
6. Distribution of languages across courses.
7. Language preferences by category.
8. Relationship between subtitle availability and viewership.
9. Top three instructors by category and sub-category based on ratings.
10. Impact of course duration on viewership.

## DAX Functions Used

### Aggregate Functions

- SUM()
- AVERAGE()
- COUNT()
- COUNTROWS()

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

## Tools and Technologies

| Tool | Purpose |
|--------|---------|
| Power BI Desktop | Dashboard Development |
| Power Query (M) | Data Cleaning and Transformation |
| DAX | Measures and Analytical Calculations |
| Microsoft Excel / CSV | Data Source |
| Data Modeling | Star Schema Implementation |

## How to Use

1. Clone the repository.

```bash
git clone https://github.com/your-username/edtech-recorded-lectures-dashboard.git
```

2. Open `EdTech_Recorded_Lectures_Analytics.pbix` in Power BI Desktop.

3. Refresh the data source if required.

4. Explore the report pages and interact with the visualizations.

## Author

Sanya Khandelwal

Data Analyst | Power BI Developer

LinkedIn: https://www.linkedin.com/in/sanya-khandelwal-data-analyst/

GitHub: https://github.com/sanyakhandelwal

## License

This project is intended for educational and portfolio purposes. The dataset was collected from publicly available EdTech platforms.
