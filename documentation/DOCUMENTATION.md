# Lumera — Digital Allyship Analytics Warehouse
## Data Science & Engineering Documentation

**Team:** Lumera — Team 39 | Women Tech Fest 2025  
**SDG:** SDG 5 — Gender Equality  
**Date:** February 2026  
**Repository:** github.com/WANGAR1/Lumera-Analytics-Dashboard  

---

## 1. Project Overview

Lumera is a Digital Allyship Toolkit designed to equip men aged 16–44 with practical, trauma-informed skills to support women experiencing gender-based violence or health-related distress.

The Data Science and Engineering team was responsible for designing and building the data infrastructure that supports analytical reporting and impact measurement for the platform. This document covers the full scope of work completed during the capstone project.

---

## 2. Problem Statement

Despite high awareness of gender-based violence among men aged 18–40, many lack the practical skills and structured guidance to respond appropriately. From a data perspective the challenge was:

- The platform was still under development during the capstone timeline
- No real user data was available to analyze
- The data science team needed to demonstrate analytical capability before the platform launched
- A full data warehouse architecture needed to be designed and implemented

---

## 3. Data Architecture

### 3.1 Medallion Architecture

The data warehouse follows the Medallion Architecture — an industry-standard approach that organizes data into three progressive layers:

| Layer | Name | Purpose |
|---|---|---|
| 🥉 Bronze | Raw Data | Stores data exactly as received — CSV files loaded into SQL Server with no transformation |
| 🥈 Silver | Cleaned Data | Removes duplicates, standardizes formats, validates score ranges, handles missing values |
| 🥇 Gold | Analytics Layer | Business-ready star schema optimized for reporting — fact and dimension tables powering dashboards |

### 3.2 Database Schema

The Bronze layer schema was designed using **dbdiagram.io** and contains five core tables:

- **lms_users** — 50 registered users (39 learners, 11 admins excluded from analysis)
- **lms_courses** — 5 learning courses across GBV awareness, communication, referral and recovery
- **lms_modules** — 15 modules, each with content_type (video or text)
- **lms_lesson_activity** — 150 activity events tracking user progress per module
- **lms_quiz_attempts** — 120 quiz attempts measuring knowledge acquisition

**Key Relationships:**
- `lms_courses.course_id` → `lms_modules.course_id`
- `lms_users.user_id` → `lms_lesson_activity.user_id`
- `lms_users.user_id` → `lms_quiz_attempts.user_id`
- `lms_modules.module_id` → `lms_lesson_activity.module_id`
- `lms_modules.module_id` → `lms_quiz_attempts.module_id`

---

## 4. Synthetic Data Generation

### 4.1 Approach & Rationale

Because the Lumera platform was still in active development, no real user data was available. The team used **Mockaroo** to generate realistic synthetic datasets that mirror what real platform data would look like.

### 4.2 Generation Order

Tables were generated in dependency order to ensure referential integrity:

1. `lms_users` — 50 rows (root anchor table)
2. `lms_courses` — 5 rows (content reference)
3. `lms_modules` — 15 rows (using course_id from Step 2)
4. `lms_lesson_activity` — 150 rows (using user_id and module_id from Steps 1 and 3)
5. `lms_quiz_attempts` — 120 rows (same ID references)

### 4.3 Admin Exclusion

11 of the 50 users were identified as admin accounts. All analytics were produced using **learner-only data (39 learners)** to ensure accurate representation of platform learning outcomes.

---

## 5. Tools & Technologies

| Tool | Purpose | Usage |
|---|---|---|
| Mockaroo | Synthetic data generation | Generated all 5 CSV datasets |
| SQL Server | Data warehouse | Hosts Bronze, Silver and Gold layers |
| dbdiagram.io | Schema design | Visual ER diagram for Bronze layer |
| Power BI Desktop | Dashboard analytics | Initial dashboard versions |
| HTML / CSS / JS | Interactive dashboards | Final production dashboards |
| GitHub | Version control | Repository for all project files |
| VS Code | Development environment | File editing and Git operations |
| Excel | Post-processing | ID prefixes and data verification |

---

## 6. Analytics Dashboards

### 6.1 Dashboard 1 — Platform Analytics

**KPIs:**
- Total Learners: **39**
- Total Activities: **111**
- Completed Activities: **41** (37% completion rate)
- Average Quiz Score: **62.9 / 100**
- Average Progress: **51.8%**

**Business Questions Answered:**
1. Which modules have the highest completion rates? → Emotional Support leads with 5 completions
2. Which user segments show the highest engagement? → Management with 37 activities
3. How effective are different content formats? → Text (65.96) outperforms Video (61.25)
4. Is knowledge improving over time? → Score trends visualized by date
5. Which modules perform best overall? → Responding to Disclosure Calmly (avg 82.0)

### 6.2 Dashboard 2 — Retention & Impact

**KPIs:**
- Return Rate: **79%** (31 of 39 learners returned)
- Average Sessions Per User: **3.2**
- Drop-off Events: **18**
- Top Quiz Score: **99.2**

**Business Questions Answered:**
1. Are users returning after learning? → Yes, 79% return rate
2. Which users create the most value? → Mariejeanne Lehr and Cornell Raikes (7 activities each)
3. What behaviour predicts completion? → Learners with 4+ activities score 8.4 points higher
4. Where do users drop off? → Self Care and Long-Term Allyship modules

### 6.3 Dashboard Versions

- **Power BI (.pbix)** — stored in `/dashboard_1` and `/dashboard_2`
- **HTML (.html)** — final production version in `/latest_dashboards`, color matched to Lumera prototype

---

## 7. Key Findings

### Learning Effectiveness
- Text modules outperform video by **4.7 points** on average (65.96 vs 61.25)
- Responding to Disclosure Calmly is the highest scoring module (**avg 82.0**)
- Emotional Support Without Overstepping has the most completions (**5**)
- Asking the Right Questions has the lowest score (**45.9**) — content may need revision

### Engagement & Retention
- **79%** of learners returned for more than one session
- Management is the most active department (**37 activities**)
- Community has low engagement (**14 activities**) — outreach opportunity
- Learners with 4+ activities score **8.4 points higher** on average

### Drop-off Analysis
- **18 drop-off events** (16% drop-off rate)
- Self Care and Safe Referral modules have the highest drop-offs
- Body Language module has **0 completions** — requires urgent content review

---

## 8. Repository Structure

```
Lumera-Analytics-Dashboard/
│
├── README.md                        ← Project overview
├── documentation/
│   ├── DOCUMENTATION.md             ← This file
│   └── Lumera_Analytics_Documentation.docx
│
├── data/
│   ├── README.md
│   ├── lms_users.csv
│   ├── lms_courses.csv
│   ├── lms_modules.csv
│   ├── lms_lesson_activity.csv
│   └── lms_quiz_attempts.csv
│
├── schema/
│   ├── README.md
│   └── lumera_bronze_schema.png
│
├── dashboard_1/
│   ├── README.md
│   └── Lumera_Dashboard1.pbix
│
├── dashboard_2/
│   ├── README.md
│   └── Lumera_Dashboard2.pbix
│
└── latest_dashboards/
    ├── README.md
    ├── Lumera_Dashboard1_Platform_Analytics.html
    └── Lumera_Dashboard2_Retention_Impact.html
```

---

## 9. Skills Demonstrated

- **Data Architecture** — Designed Medallion Architecture for a real analytics use case
- **Database Schema Design** — Created relational schema with PKs and FKs using dbdiagram.io
- **Synthetic Data Engineering** — Generated multi-table datasets with referential integrity
- **Data Cleaning** — Identified and excluded admin accounts, validated data types
- **ETL Pipeline Design** — Planned Extract, Transform, Load pipeline from CSV to SQL Server
- **Business Analytics** — Translated business questions into measurable KPIs
- **Dashboard Development** — Built dashboards in both Power BI and HTML/CSS/JavaScript
- **Version Control** — Organized and maintained project repository on GitHub
- **Data Storytelling** — Communicated findings through visual dashboards

---

## 10. Limitations & Future Work

### Current Limitations
- All data is synthetic — findings reflect simulated patterns, not real user behaviour
- Survey tables (confidence pre/post assessment) were designed but not populated
- Silver and Gold layer SQL scripts not completed within capstone timeline

### Recommended Next Steps
- Connect dashboards to live SQL Server when platform launches
- Build Silver layer cleaning scripts in Python (Pandas)
- Implement Gold layer star schema for optimized querying
- Add confidence assessment tracking using survey data
- Set up automated KPI refresh schedule

---

*Lumera · Team 39 · Women Tech Fest 2025 · SDG 5 — Gender Equality*
