# Excel Project 2: Analyzing Data Science Job Listings

## Introduction

This project is part of my transition into the field of data analytics. As someone coming from a marketing background, I wanted to sharpen my technical skills in Excel and use real-world data to uncover actionable insights.

The dataset—provided by Luke Barousse in his Excel for Data Analytics course—includes 2023 job listings for data roles across multiple countries. It captures information such as job title, salary, country, and required skills.

In this project, I aim to answer four core questions:
1. Do more skills lead to higher salaries?
2. How does salary differ by region?
3. What are the top skills data professionals need?
4. Which skills offer the best mix of pay and demand?

Tools used: Power Query, Power Pivot, DAX, PivotTables, PivotCharts, and Slicers.

## Queries Created

To prepare the data for analysis, I used Power Query to create two separate queries from the original Excel file:

### `data_jobs_salary` (Main Jobs Table)

Transformations included:
* Changed column data types
* Cleaned the `job_via` column by replacing "via " with blank
* Extracted Month and Date from `job_posted_datetime`
  * **Month** (for aggregation)
  * **Date** (for relationship building)
* Reorganized columns for clarity
* Added an `Index` column to serve as a unique job identifier

### `data_job_skills` (Skills Table)

Created by referencing `data_jobs_salary` and transformed as follows:
* Removed all columns except `Index` and `skills`
* Split the `skills` column by delimiter (", ")
* Unpivoted the resulting columns into individual skill rows
* Trimmed excess whitespace
* Corrected capitalization for each skill name

Both queries were loaded to the **Data Model** and connected via the `job_id` index to enable skill-to-job mapping in PivotTables and DAX.

## Question 1: Do more skills get you better pay?

### Approach

* Created a scatter plot: Median Salary (X-axis) vs. Average Skills Requested Per Job Posting (Y-axis)
* Used explicit DAX measures:
  * **Median Salary**: `=MEDIAN(data_jobs_salary[salary_year_avg])`
  * **Skills Per Job**: `=DIVIDE([Skill Count], [Job Count])`
* Applied a Country slicer and selected United States

### Insights

* Positive correlation: More skills are generally associated with higher salaries
* Roles like **Business Analyst**, which list fewer skills, tend to pay less
* Specialization appears to drive salary growth

## Question 2: What's the median salary for data jobs in different regions?

### Approach

* Built a PivotTable using:
  * **Rows**: `job_title_short`
  * **Values**:
    * Median Salary
    * US Median Salary: `=CALCULATE([Median Salary], data_jobs_salary[job_country] = "United States")`
    * Non-US Median Salary: `=CALCULATE([Median Salary], data_jobs_salary[job_country] <> "United States")`
  * **Slicer**: Country

### Insights

* US salaries are generally higher than Non-US across job titles
* Some job titles (e.g. Senior Data Scientist and Senior Data Engineer) have high median salaries globally
* Countries like El Salvador show fewer job titles, indicating a smaller market or a smaller dataset sample for that country.
* These insights are useful for setting expectations in career planning and for salary benchmarking in global job markets

## Question 3: What are the top skills of data professionals?

### Approach

* Created a horizontal bar chart using:
  * **Rows**: `job_skills`
  * **Values**: Skill Likelihood = `=DIVIDE([Skill Count], [Job Count])`
  * Slicers: Job Title (Data Analyst) and Country (United States)

#### Skill Likelihood Results

* SQL: 53%
* Excel: 41%
* Tableau: 29%
* Python: 28%
* SAS: 19%
* Power BI: 17%
* R: 16%
* Word: 10%
* PowerPoint: 10%
* Oracle: 7%

### Insights

* SQL and Excel are the most frequently requested skills for US Data Analyst roles
* Skill Likelihood percentages represent demand across all job postings (not a total of 100% because a single job can list multiple skills)
* Knowing which skills are most valued based on actual market demand (not just assumptions) helps job seekers prioritize their learning path

## Question 4: What’s the pay of the top 10 skills?

### Approach

* Built a combo PivotChart with:
  * Clustered columns: Median Salary per Skill
  * Line with markers: Skill Likelihood (%)
* Filtered to Top 10 skills by Skill Likelihood
* Used DAX and the `CROSSFILTER()` function to bring salary data into the skill table:
  * **Median Salary by Skill**: `=CALCULATE([Median Salary], CROSSFILTER(data_jobs_salary[job_id], data_jobs_skills[job_id], Both))`

#### Sample Data (Top 10 Skills)

| Skill      | Median Salary | Skill Likelihood |
| ---------- | ------------- | ---------------- |
| Python     | \$97,087      | 28%              |
| Oracle     | \$96,924      | 7%               |
| Tableau    | \$92,500      | 29%              |
| Power BI   | \$90,000      | 17%              |
| SQL        | \$90,000      | 53%              |
| SAS        | \$90,000      | 19%              |
| R          | \$90,000      | 16%              |
| PowerPoint | \$85,000      | 10%              |
| Excel      | \$84,500      | 41%              |
| Word       | \$81,682      | 10%              |

### Insights

* Python, Oracle, and Tableau are associated with higher salaries
* SQL has the highest demand, with solid median pay
* Skills like Word and PowerPoint are less valued in high-paying roles
* This chart helps visualize where to invest time based on salary and demand

## Conclusion

I took on this project to sharpen my Excel skills as I transition into the field of data analytics. At the same time, I wanted to better understand the data job market—specifically which roles are in demand and which skills are most valued.

Throughout the project, I learned how to use Power Query to clean and transform raw data, Power Pivot to build relationships between tables, and DAX to create meaningful measures. I also worked with PivotTables, combo charts, and slicers to uncover trends and make insights more interactive.

One insight that genuinely surprised me was the overwhelming demand for SQL over Excel. Coming from a marketing background, I had always assumed Excel was the go-to tool for analysts. This project helped me challenge that assumption and focus on the skills employers are actually looking for.

Moving forward, I plan to prioritize high-demand skills like SQL and Excel. Once I’ve built a strong foundation, I’ll likely expand into tools like Python or Tableau to round out my technical toolkit.
