# EdTech Startup — Recorded Lectures Analytics Dashboard

A Power BI dashboard built for an EdTech startup to analyse online course data scraped from multiple EdTech platforms. The project covers end-to-end data analysis: cleaning raw data, building a relational data model, authoring DAX measures, and delivering a category-centric interactive dashboard.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Business Questions Answered](#business-questions-answered)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Data Cleaning](#data-cleaning)
- [Data Model](#data-model)
- [DAX Measures](#dax-measures)
- [Dashboard Pages](#dashboard-pages)
- [Screenshots](#screenshots)
- [How to Open the Report](#how-to-open-the-report)
- [Tools Used](#tools-used)

---

## Project Overview

The client is an EdTech startup looking to expand its recorded lecture catalogue. They collected course data from various EdTech platforms and required a structured analysis to understand:

- Which categories and sub-categories attract the most viewers
- Which languages and subtitle combinations drive engagement
- How course duration, type, and instructor quality affect viewership
- Where to focus content investment for maximum return

The dashboard is designed with a **category-first perspective** throughout, enabling the client to make data-driven decisions at a granular level.

---

## Business Questions Answered

| # | Question |
|---|----------|
| 1 | Total number of courses in the catalogue |
| 2 | Average course rating across all courses |
| 3 | Total and average number of views across all courses |
| 4 | Distribution of course types within each category and sub-category |
| 5 | Average views by category, sub-category, and language |
| 6 | Distribution of languages in which courses are created |
| 7 | Language preferences for the top 5 categories by viewer count |
| 8 | Relationship between subtitle availability and number of views |
| 9 | Top 3 instructors per category and sub-category based on ratings (static visual) |
| 10 | Relationship between course duration and views, by category and sub-category |

---

## Dataset

**File:** `data/Online_Courses.csv`

Raw data collected from multiple EdTech platforms (Coursera, edX, Udemy, FutureLearn, etc.).

| Column | Description |
|--------|-------------|
| Title | Course name |
| URL | Course link |
| Short Intro | Brief description of the course |
| Category | Top-level subject category |
| Sub-Category | More specific subject area |
| Course Type | Specialization, Professional Certificate, Course, etc. |
| Language | Primary language of instruction |
| Subtitle Languages | Available subtitle languages |
| Skills | Skills taught in the course |
| Instructors | Instructor name(s) |
| Rating | Course rating (e.g., 4.7stars) |
| Number of viewers | Total viewer count |
| Duration | Course duration (text, e.g., "Approximately 3 months") |
| Site | Source platform |

> **Note:** The dataset required significant cleaning before analysis — see [Data Cleaning](#data-cleaning).

---

## Project Structure

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
---

## Data Cleaning

The following transformations were applied in Power Query before modelling:

- **Rating column** — Stripped the `stars` suffix and converted to decimal number
- **Number of viewers** — Removed commas and whitespace; cast to integer
- **Duration column** — Parsed text values into numeric hours using custom logic:
  - 1 month = 60 hours of content
  - Flexible schedule = 200 hours
- **Instructors** — Split multi-value instructor strings and exploded into a separate table for instructor-level analysis
- **Subtitle Languages** — Cleaned prefix text ("Subtitles:") and normalised to a usable format
- **Nulls and blanks** — Handled missing values in Rating, Viewers, Duration, and Category columns
- **Duplicate rows** — Identified and removed exact duplicates
- **Category / Sub-Category casing** — Standardised text casing for consistent grouping

---

## Data Model

The report is built on a **single fact table** with duplicate tables created where DAX filter context required an isolated copy.

### Base Table

| Table | Description |
|-------|-------------|
| `Online_Course` | Single source of truth — all cleaned course records. Every visual and measure is built on this table unless a duplicate was required. |

### Duplicate Tables

Duplicate tables were created from `Online_Course` to resolve filter context conflicts in specific questions. Each duplicate is used only for its designated question and is not connected to the main table via relationships.

| Table | Used For | Purpose |
|-------|----------|---------|
| `Subtitle Count Table` | Question 8 | Isolates subtitle availability data to analyse its relationship with viewer count without cross-filter interference from other slicers |
| `Tutors and Ratings` | Questions 9 & 10 | Enables instructor-level ranking by rating per category and sub-category, and duration vs views analysis, without circular dependency on the main table |

### Design Rationale

- The base `Online_Course` table handles the majority of questions (1–7) directly via DAX measures using `ALL`, `ALLEXCEPT`, and `CALCULATE`
- Duplicate tables are a deliberate modelling choice — not redundancy — used where applying `RANKX` or aggregating across a different grain would produce incorrect results in the original table's filter context
- No star schema or additional dimension tables were introduced to keep the model simple and maintainable

---

## DAX Measures

Key measures authored for the report:

```dax
-- Total Courses
Total Courses = COUNTROWS(Courses)

-- Average Rating
Avg Rating = AVERAGE(Courses[Rating])

-- Total Views
Total Views = SUM(Courses[Number of viewers])

-- Average Views
Avg Views = AVERAGE(Courses[Number of viewers])

-- Avg Views by Category (ignores sub-filters)
Avg Views Category =
CALCULATE(
    AVERAGE(Courses[Number of viewers]),
    ALLEXCEPT(Courses, Courses[Category])
)

-- Top 5 Categories by Views flag
Top 5 Category Flag =
VAR RankVal =
    RANKX(
        ALL(Courses[Category]),
        CALCULATE(SUM(Courses[Number of viewers])),
        ,
        DESC,
        DENSE
    )
RETURN IF(RankVal <= 5, 1, 0)

-- Top 3 Instructors per Category
Instructor Rank =
RANKX(
    ALLEXCEPT(Instructors, Instructors[Category]),
    CALCULATE(AVERAGE(Courses[Rating])),
    ,
    DESC,
    DENSE
)
```

> Additional measures using `CALCULATE`, `ALL`, `ALLEXCEPT`, `RANKX`, `FILTER`, `TOPN`, and `SWITCH` are documented inline within the `.pbix` file.

---

## Dashboard Pages

| Page | Key Visuals |
|------|-------------|
| **Overview** | KPI cards — Total Courses, Avg Rating, Total Views, Avg Views |
| **Category Analysis** | Course count by category/sub-category; course type distribution matrix |
| **Language & Subtitles** | Language distribution donut; Top 5 category language preference; Subtitle vs Views bar |
| **Engagement by Duration** | Scatter plot of Duration (hours) vs Views by category and sub-category |
| **Top Instructors** | Static table — Top 3 instructors per category and sub-category by rating |

---

## Screenshots

Screenshots of all dashboard pages are available in the [`dashboard_screenshots/`](./dashboard_screenshots/) folder.

---

## How to Open the Report

1. Download and install [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
2. Clone or download this repository
3. Open `EdTech_Recorded_Lectures_Analytics.pbix` in Power BI Desktop
4. If prompted about data source paths, update the CSV path to your local `data/Online_Courses.csv`
5. Click **Refresh** to reload the data

> Built on Power BI Desktop (June 2025 or later). Earlier versions may not support all visual types used.

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Power BI Desktop | Data modelling, DAX, dashboard design |
| Power Query (M) | Data ingestion, cleaning, transformation |
| DAX | Measures, calculated columns, ranking logic |
| CSV | Source dataset |

## Author

Sanya Khandelwal

Data Analyst | Power BI Developer

LinkedIn: https://www.linkedin.com/in/sanya-khandelwal-data-analyst/

GitHub: https://github.com/sanyakhandelwal

## License

This project is intended for educational and portfolio purposes. The dataset was collected from publicly available EdTech platforms.
