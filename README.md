# Introduction
Dive into the data job market! Focusing on data analyst roles, this project explores top-paying jobs, in-demand skills, where high demand meets high salary in data analytics.

SQL queries ? check them out here : [project_sql Folder](/project_sql/)

# Background
1. What are the top-paying data analyst jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for data analysts?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn?

# Tools i used 
For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **SQL:** The backbone of my analysis, allowing me to query the database and unearth critical insights.

- **PostgreSQL:** The chosen database management system, ideal for handling the job posting data.

- **Visual Studio Code:** My go-to for database management and executing SQL queries.

- **Git & GitHub:** Essential for version control and sharing my SQL scripts and analysis, ensuring collaboration and project tracking.

# Analysis
Each query for this project aimed at investigating specific aspects of the data analyst job market. Here's how I approached each question:

### 1. Top Paying Data Analyst Jobs

To identify the highest-paying roles, I filtered data analyst positions by average yearly salary and location, focusing on remote jobs. This query highlights the high paying opportunities in the field.
```sql
 select job_id,
 job_title,
 job_location,
 company_dim.name as company_name,
 job_schedule_type,
 salary_year_avg,
 job_posted_date
 FROM job_postings_fact 
 left JOIN company_dim ON job_postings_fact.company_id=company_dim.company_id
 where job_title_short='Data Analyst' and job_location='Anywhere'
 and salary_year_avg is NOT NULL
 ORDER BY 
 salary_year_avg DESC
 LIMIT 10
 ```
 Here's the breakdown of the top data analyst jobs in 2023:

- **Wide Salary Range:** Top 10 paying data analyst roles span from $184,000 to $650,000, indicating significant salary potential in the field.

- **Diverse Employers:** Companies like SmartAsset, Meta, and AT&T are among those offering high salaries, showing a broad interest across different industries.

- **Job Title Variety:** There's a high diversity in job titles, from Data Analyst to Director of Analytics, reflecting varied roles and specializations within data analytics.

### 2. Skills for Data Analyst Jobs
Question : what skills are required for top paying data analyst jobs?

why? it will give you the skills required for the highest paying jobs as data analyst.
```sql
with top_paying_jobs as (
 select job_id,
 job_title,
 job_location,
 company_dim.name as company_name,
 job_schedule_type,
 salary_year_avg
 FROM job_postings_fact 
 left JOIN company_dim ON job_postings_fact.company_id=company_dim.company_id
 where job_title_short='Data Analyst' and job_location='Anywhere'
 and salary_year_avg is NOT NULL
 ORDER BY 
 salary_year_avg DESC
 LIMIT 10
)
SELECT top_paying_jobs.*,
skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id=skills_job_dim.job_id 
INNER JOIN skills_dim ON skills_job_dim.skill_id=skills_dim.skill_id 
```
- High-paying Data Analyst jobs often require multiple technical skills, showing that employers expect analysts to work with several tools rather than just one.

- Core data skills like SQL, Python, and visualization tools frequently appear in top-salary roles, indicating these are highly valuable skills in the market.

- Analyzing the skills linked to the highest-paying jobs helps identify which skills can increase earning potential for Data Analysts.
### 3. Top skills in demand for Data Analyst Jobs
Question : what are the most in demand skills for data analyst job?

The query gives you the most demanded skills for data analyst job
```sql
select skills_dim.skill_id,
skills,
 count(skills_job_dim.job_id) count_of_skills
FROM job_postings_fact
INNER JOIN skills_job_dim ON skills_job_dim.job_id=job_postings_fact.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id=skills_dim.skill_id
where job_title_short='Data Analyst' AND job_work_from_home=true
GROUP BY
skills,skills_dim.skill_id
ORDER BY count_of_skills DESC
LIMIT 5
```
- Certain skills appear much more frequently in remote Data Analyst job postings, indicating they are the most in-demand skills for work-from-home roles.

- Core technical skills dominate the top results, showing that employers prioritize strong data analysis, querying, and visualization abilities for remote analysts.

- Focusing on the top 5 most requested skills can significantly improve a candidate’s chances of qualifying for remote Data Analyst positions.
### 4. skills with highest salary for Data Analyst Jobs
Question : what are the top skills based on salary ?

Why ? it helps you identify the most rewarding skills to aquire as a data analyst
```sql
select skills_dim.skill_id,
skills,
 round(avg(salary_year_avg),0) salary_avg
FROM job_postings_fact
INNER JOIN skills_job_dim ON skills_job_dim.job_id=job_postings_fact.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id=skills_dim.skill_id
where job_title_short='Data Analyst' 
and salary_year_avg IS NOT NULL
--AND job_work_from_home=true
GROUP BY
skills,skills_dim.skill_id
ORDER BY salary_avg DESC
LIMIT 100
```
- Some skills are associated with higher average salaries, showing that certain technical tools are more valuable in the data analyst job market.

- Advanced or specialized skills tend to appear near the top of the salary ranking, suggesting that deeper technical expertise can lead to better-paying roles.

- Understanding which skills have the highest average salary helps professionals prioritize learning skills that can increase their earning potential.

### 5. most optimal skills for Data Analyst Jobs

Question : what are the most optimal skills to learn for data analyst?

optimal : high paying and high demanding
```sql
with optimalskill as (
    select skills_job_dim.skill_id as skillid,
skills,
count(skills_job_dim.job_id) as skill_count_demand,
round(avg(salary_year_avg),0) Avg_salary
from job_postings_fact
inner join skills_job_dim on job_postings_fact.job_id=skills_job_dim.job_id
INNER join skills_dim on skills_job_dim.skill_id=skills_dim.skill_id
where job_title_short='Data Analyst'
and salary_year_avg IS NOT NULL
and job_work_from_home=true

GROUP BY
skills,skills_job_dim.skill_id
ORDER BY Avg_salary DESC
)
SELECT * FROM optimalskill
WHERE skill_count_demand >10
LIMIT 25
```
- Skills that appear frequently in job postings and also have high average salaries represent the most valuable skills for remote Data Analyst roles.

- Filtering for skills with demand greater than 10 ensures the results focus on skills that are both well-paid and widely required by employers.

- This analysis helps identify the optimal skills to learn, as they offer a strong combination of high market demand and higher salary potential.

# What i learned
Throughout this adventure, I've turbocharged my SQL toolkit with some serious firepower:

- **Complex Query Crafting:** Mastered the art of advanced SQL, merging tables like a pro and wielding WITH clauses for ninja-level temp table maneuvers.

- **Data Aggregation:** Got cozy with GROUP BY and turned aggregate functions like COUNT() and AVG () into my data-summarizing sidekicks.

- **Analytical Wizardry:** Leveled up my real-world puzzle-solving skills, turning questions into actionable, insightful SQL queries.
# Conclusion
This project enhanced my SQL skills and provided valuable insights into the data analyst job market. The findings from the analysis serve as a guide to prioritizing skill development and job search efforts. Aspiring data analysts can better position themselves in a competitive job market by focusing on high-demand, high-salary skills. This exploration highlights the importance of continuous learning and adaptation to emerging trends in the field of data analytics
