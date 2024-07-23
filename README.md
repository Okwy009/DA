# OVERVIEW
Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from Luke Barousse's Python Course which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions
Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)

## Data Preparation and Cleanup
This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data
I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.
```Python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```
## Filter US Jobs
To focus my analysis on the U.S. job market, I apply filters to the dataset, narrowing down to roles based in the United States.
```Python
df_US = df[df['job_country'] == 'United States']

```

# Tools I Used
For my deep dive into the data analyst job market, I harnessed the power of several key tools:

1. **Python**: The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
    - Pandas Library: This was used to analyze the data.
    - Matplotlib Library: I visualized the data.
    - Seaborn Library: Helped me create more advanced visuals.
2. **Jupyter Notebooks**: The tool I used to run my Python scripts which let me easily include my notes and analysis.
3. **Visual Studio Code**: My go-to for executing my Python scripts.
4. **Git & GitHub**: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.


# The Analysis
## 1. What are the most demanded skills for the top 3 most popular data roles?

Finding the most demanded skills for the top 3 most popular data roles. I filtered the positions by getting the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View detailed steps on my notebook here: [02_Skill_Demand](3_Project\02_Skill_Demand.ipynb).

### Visualization
```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```
### Result
![Likelihood of Skills Requested in the US Job Postings](3_Project\images\Likelihood_of_Skills_Requested_in_the_US_Job_Postings.png)

### Insights:
- SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for both roles. For Data Engineers, Python is the most sought-after skill, appearing in 68% of job postings.

- Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).

- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%).

## 2. How are in-demand skills trending for Data Analysts?
To analyze the trending skills for Data Analysts in 2023, I filtered job postings for data analyst positions and grouped the skills by the month they were listed. This allowed me to identify the top 5 skills for data analysts each month, highlighting their popularity throughout the year.

View my notebook with detailed steps here: [3_Skills_Trend](3_Project\03_Skills_Trend.ipynb).

## Visualization
```python

from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()

```

## Result
![Trending Top Skills for Data Analysts in the US](3_Project\images\Trending_Top_Skills_for_Data_Analysts_in_the_US.png)


## Insights
- SQL continues to be the most in-demand skill, consistently appearing in job postings throughout the year. This underscores its essential role in data analysis.
- Tableau's demand has surged, particularly in the latter part of the year, overtaking both Python and Excel. This indicates an increasing preference for sophisticated data visualization tools.
- Excel’s demand remains steady, fluctuating around 40%, which underscores its ongoing significance even as more specialized tools gain popularity.
- Python's popularity is on the rise, starting lower but steadily increasing throughout the year. By the end of the year, it surpasses Excel, highlighting its expanding role in data analysis and machine learning.

## 3.  How well do jobs and skills pay for Data Analysts?
To identify the highest-paying roles and skills, I focused on jobs in the United States and analyzed their median salaries. Initially, I examined the salary distributions for common data roles such as Data Scientist, Data Engineer, and Data Analyst to determine which positions offer the highest pay.

View my notebook with detailed steps here: [4_Salary_Analysis](3_Project\04_Salary_Analysis.ipynb).

## Visualization
```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()

```

## Result
![Salary Distributions of Data Jobs in the US](3_Project\images\Salary_Distributions_of_Data_Jobs_in_the_US.png)

## Insights
- Salary ranges vary significantly across different job titles. Senior Data Scientist roles often offer the highest salary potential, reaching up to $600K. This highlights the high value placed on advanced data skills and industry experience.
- Senior Data Engineer and Senior Data Scientist roles exhibit a significant number of high-end salary outliers, indicating that exceptional skills or unique circumstances can result in substantial pay. Conversely, Data Analyst roles tend to have more consistent salaries with fewer outliers.

### Highest Paid & Most Demanded Skills for Data Analysts
Next, I refined my analysis to focus exclusively on data analyst roles. I examined the highest-paid skills and the most in-demand skills, presenting my findings using two bar charts.

### Visualization
```Python

fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analystsr')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()
```

### Result
![The Highest Paid & Most In-Demand Skills for Data Analysts in the US](3_Project\images\The_Highest_Paid_and_Most_In-Demand_Skills_for_Data_Analysts_in_the_US.png)

### Insights:
- The top graph indicates that specialized technical skills such as **dplyr**, **Bitbucket**, and **Gitlab** are linked to higher salaries, with some reaching up to $200K. This suggests that advanced technical proficiency can significantly boost earning potential.
- The bottom graph emphasizes that foundational skills such as **Excel**, **PowerPoint**, and **SQL** are the most sought-after, despite not offering the highest salaries. This underscores the critical role these core skills play in securing employment in data analysis roles.
- There is a noticeable difference between the highest-paid skills and the most in-demand skills. Data analysts looking to enhance their career prospects should aim to develop a diverse skill set that encompasses both high-paying specialized skills and widely demanded foundational skills.

## 4. What are the most optimal skills to learn for Data Analysts?

To pinpoint the most optimal skills to learn—those that are both highly paid and in high demand—I calculated the percentage of skill demand and the median salary for each skill. This approach makes it easier to identify the most valuable skills to focus on.

View my notebook with detailed steps here: [5_Optimal_Skills](3_Project\05_Optimal_Skills.ipynb).

## Visualization
```Python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()
```
## Result
![Most Optimal Skills for Data Analysts in the US](3_Project\images\Most_Optimal_Skills_for_Data_Analysts_in_the_US.png)

## Insights
- **Oracle** stands out with a median salary of nearly $97K, even though it is less frequently mentioned in job postings. This indicates a significant value placed on specialized database skills within the data analyst field.
- Skills like **Excel** and **SQL** are frequently mentioned in job listings but tend to offer lower median salaries. In contrast, specialized skills such as **Python** and **Tableau** not only command higher salaries but are also moderately common in job postings.
- Skills like **Python**, **Tableau**, and **SQL Server** are not only among the higher-paying options but are also fairly common in job listings. This suggests that proficiency in these tools can open up excellent opportunities in Data Analytics.

### Visualizing Different Techonologies
Visualizing different technologies as well in the graph. We'll add color labels based on the technology.

### Visualization
```Python
from matplotlib.ticker import PercentFormatter


scatter = sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology',  
    palette='bright',  
    legend='full' 
)
plt.show()

```

### Result
![Most Optimal Skills for Data Analysts in the US with Coloring by Technology](3_Project\images\Most_Optimal_Skills_for_Data_Analysts_in_the_US_with_Coloring_by_Technology.png)

### Insights
- The scatter plot reveals that **programming** skills (blue color) generally cluster at higher salary levels compared to other categories. This suggests that expertise in **programming** can lead to greater salary benefits within the data analytics field.
- **Database skills**, such as Oracle and SQL Server (orange color), are linked to some of the highest salaries among data analyst tools. This highlights the substantial demand and value placed on expertise in data management and manipulation within the industry.
- **Analyst tools** (green color), including Tableau and Power BI, are prevalent in job postings and offer competitive salaries, showing that visualization and data analysis software are crucial for current data roles. This category not only has good salaries but is also versatile across different types of data tasks.

# What I Learned
Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

Advanced Python Usage: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
Data Cleaning Importance: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
Strategic Skill Analysis: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.
# Insights
This project provided several general insights into the data job market for analysts:
- There is a strong correlation between the demand for specific skills and the salaries they command. Advanced and specialized skills, such as Python and Oracle, frequently result in higher salaries.
- The demand for specific skills is constantly evolving, reflecting the dynamic nature of the data job market. Staying updated with these trends is crucial for career advancement in data analytics.
- Understanding the economic value of skills—those that are both in-demand and well-compensated—can help data analysts prioritize their learning to maximize their financial returns.

# Challenges I Faced
- Addressing data inconsistencies:
    
    Data inconsistencies and missing entries demands meticulous attention and robust data-cleaning methods to maintain the integrity of your analysis.
- Complex Data Visualization:

    Crafting effective visual representations of complex datasets was challenging, yet crucial for clearly and compellingly conveying insights.
- Balancing Breadth and Depth:

    Balancing the depth of each analysis while maintaining a broad overview of the data landscape required constant effort to ensure comprehensive coverage without getting lost in the details.


# Conclusion
This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.