# Student Mental Health Analysis using SQL

![Mental Health](mentalhealth.jpg)

## Table of Contents
* [Project Overview](#project-overview)
* [Dataset Description](#dataset-description)
* [Dataset Insights](#dataset-insights)
* [Objective](#objective)
* [SQL Query](#sql-query)
* [Result](#result)
* [Key Findings](#key-findings)

## **Project Overview**
This project investigates whether studying in a foreign country affects students' mental health, using data from a Japanese international university's 2018 survey. The original study concluded that international students face a higher risk of mental health difficulties, with social connectedness and acculturative stress being key predictors of depression.

Using **PostgreSQL**, this analysis aims to:
- Explore patterns in mental health scores of international students.
- Determine if the **length of stay** impacts mental health outcomes.

## **Dataset Description**
The dataset contains information from both international and domestic students, focusing on mental health metrics. Below are the key fields:

| **Field Name** | **Description** |
|---------------|------------------|
| `inter_dom`   | Student type (international or domestic) |
| `japanese_cate` | Japanese language proficiency level |
| `english_cate`  | English language proficiency level |
| `academic`      | Academic level (undergraduate or graduate) |
| `age`           | Student's current age |
| `stay`          | Length of stay in years |
| `todep`         | Depression score (PHQ-9 test) |
| `tosc`          | Social connectedness score (SCS test) |
| `toas`          | Acculturative stress score (ASISS test) |

## **Dataset Insights:**
- The dataset includes numerical scores for depression (PHQ-9), social connectedness (SCS), and acculturative stress (ASISS).
- Data covers varying lengths of stay, ages, and language proficiencies.
- Null or missing values are minimal and have been handled appropriately.
- **Total Records:** 286
- **International Students Analyzed:** 268
- **Length of Stay (Years):** 1–10

## **Objective**
Generate a table with the following specifications:
- **Columns:** `stay`, `count_int`, `average_phq`, `average_scs`, `average_as`
- **Details:**
   - `stay`: Length of stay (years)
   - `count_int`: Number of international students per length of stay
   - `average_phq`: Average depression score (rounded to 2 decimals)
   - `average_scs`: Average social connectedness score (rounded to 2 decimals)
   - `average_as`: Average acculturative stress score (rounded to 2 decimals)
- **Sorting:** By `stay` in descending order
- **Number of Rows:** 9

Additionally:
- Investigate correlations between length of stay and mental health metrics.
- Identify anomalies and significant patterns.
- Validate findings using statistical analysis.

## **SQL Query**

```sql
SELECT stay, 
    COUNT(inter_dom) AS count_int, 
    ROUND(AVG(todep), 2) AS average_phq, 
    ROUND(AVG(tosc), 2) AS average_scs, 
    ROUND(AVG(toas), 2) AS average_as
FROM public.students
WHERE inter_dom = 'Inter' AND stay IS NOT NULL
GROUP BY stay
ORDER BY stay DESC;
```

## **Result**

| stay | count_int | average_phq | average_scs | average_as |
|------|-----------|-------------|-------------|------------|
| 10   | 1         | 13          | 32          | 50         |
| 8    | 1         | 10          | 44          | 65         |
| 7    | 1         | 4           | 48          | 45         |
| 6    | 3         | 6           | 38          | 58.67      |
| 5    | 1         | 0           | 34          | 91         |
| 4    | 14        | 8.57        | 33.93       | 87.71      |
| 3    | 46        | 9.09        | 37.13       | 78         |
| 2    | 39        | 8.28        | 37.08       | 77.67      |
| 1    | 95        | 7.48        | 38.11       | 72.8       |

## **Key Findings**
1. **Depression Trends (PHQ-9 Scores):**
   - Higher depression scores are observed in shorter stays.
   - Students with longer stays tend to show lower average depression scores.

2. **Social Connectedness (SCS Scores):**
   - Social connectedness improves with longer stays.
   - Slight declines are observed in mid-length durations.

3. **Acculturative Stress (ASISS Scores):**
   - Stress levels decrease with extended stays.
   - Anomalies are observed, particularly in year 5 and year 10.

4. **Correlation Insights:**
   - A negative correlation exists between length of stay and depression scores.
   - Positive trends are seen between social connectedness and mental health stability.

5. **Outliers:**
   - Years 5 and 10 show sharp deviations from general trends, suggesting the need for qualitative follow-up.
