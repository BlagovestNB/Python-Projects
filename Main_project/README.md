## Overview

The project analyzes the data job market, focusing on data analyst roles. This project was created to practice my programming skills using the libraries pandas, matplotlib, and seaborn. The project is divided into top-paying and in-demand skills to help identify optimal job opportunities for data analysts.

The data comes from [Luke Barousse`s Python Course](https://github.com/lukebarousse/Python_Data_Analytics_Course) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explored key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

## Main Questions

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for data analysts?
3. How well do jobs and skills pay for data analysts in the period (Jan 2023 – Jan 2024)?
4. What are the optimal skills for a data analyst to learn?

## Tools Used

For my deep dive into the data analyst job market, I harnessed the power of several key tools:

1. Python: The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
* Pandas Library: This was used to analyze the data. 
* Matplotlib Library: I visualized the data.
* Seaborn Library: Helped me create more advanced visuals.
2. Jupyter Notebooks: The tool I used to run my Python scripts which let me easily include my notes and analysis.
3. Visual Studio Code: My go-to for executing my Python scripts.
4. Git & GitHub: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

## Import & Clean Up Data
I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python
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
## The Analysis
Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:

## 1. What are the most demanded skills for the top 3 most popular data roles?
To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here: [Skills on demand](https://github.com/BlagovestNB/Python-Projects/blob/eddff5e0ceb1a22f0b0da512c0e2a34741a39758/Main_project/1.%20Skills%20on%20Demand.ipynb)

### Visualize Data

```python
for i, job in enumerate(job_title):
    df_plot = (
        df_skills[df_skills['job_title_short'] == job]
        .sort_values(by = 'skill_count', ascending = False)
        .head(5) 
    ) 
    
    sns.barplot(
        data = df_plot,
        y= 'job_skills',
        x= 'skill_count',
        ax = ax[i],
        legend= False,
        hue= 'skill_count',
        palette="dark:b_r"
    ) 
```
### Result
![Alt text](https://github.com/BlagovestNB/Python-Projects/blob/749b96b87100b1d64727be8586ebf31b346a3ff1/Main_project/Pictures/output.png)

### Insights:
* SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for both roles. For Data Engineers, Python is the most sought-after skill, appearing in 68% of job postings.
* Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).
* Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%).

## 2. How are in-demand skills trending for Data Analysts?
To find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here: [Skills Trending](https://github.com/BlagovestNB/Python-Projects/blob/134d92da6324e84f5fd49f8dcb4d76d2bb2cd028/Main_project/2.%20Skills%20Trending.ipynb)

Visualize Data
```python
for i, skill in enumerate(df_percent):

    if i == 3:
        plt.text(11, 0.16, skill)
    
    else:
        plt.text(11, df_percent.iloc[-1, i], skill)

sns.set_theme(style="ticks")
sns.lineplot(data = df_percent, dashes = False, legend= False,)

ax = plt.gca()
ax.yaxis.set_major_formatter((plt.FuncFormatter(lambda y, pos: f'{y*100:.2f}%')))
```

### Results
![Alt text](https://github.com/BlagovestNB/Python-Projects/blob/4010a8fc9c3929de7e5b28d7454d65612a409566/Main_project/Pictures/Skills%20Trending%20Final.png)
### Insights:
* SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand.
* Excel experienced a significant increase in demand starting around September, surpassing both Python and Tableau by the end of the year.
* Both Python and Tableau show relatively stable demand throughout the year with some fluctuations but remain essential skills for data analysts. Power BI, while less demanded compared to the others, shows a slight upward trend towards the year's end.

## 3. How well do jobs and skills pay for Data Analysts?
To identify the highest-paying roles and skills, I only got jobs in the United States and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most.

View my notebook with detailed steps here: [Salary Analysis](https://github.com/BlagovestNB/Python-Projects/blob/730ebac33f44f7ac3874e2cf3b5b4eb9a7ac6b82/Main_project/3.%20Salaty%20Analysis.ipynb)

Visualize Data
```python
sns.boxplot(
    data=df_title,
    x='salary_year_avg',
    y='job_title_short',
    order=df_jobs.index
)

ax = plt.gca()
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))
```
### Results
![Alt text](https://github.com/BlagovestNB/Python-Projects/blob/e40cef1c9e5949e386b11b1f06dcd35ed707e890/Main_project/Pictures/3.%20Salary%20Distributions.png)

### Insights:
* There's a significant variation in salary ranges across different job titles. Senior Data Scientist positions tend to have the highest salary potential, with up to $600K, indicating the high value placed on advanced data skills and experience in the industry.
* Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles. In contrast, Data Analyst roles demonstrate more consistency in salary, with fewer outliers.
* The median salaries increase with the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.

### Highest Paid & Most Demanded Skills for Data Analysts
Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.
### Visualize Data
```python
fig, ax = plt.subplots(2, 1)  

sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')
```
### Results
Here's the breakdown of the highest-paid & most in-demand skills for data analysts in the US:

![Alt text](https://github.com/BlagovestNB/Python-Projects/blob/1103f2d38f4871d6ce7540a71d640b844c5da824/Main_project/Pictures/3.1%20Salary%20Distributions.png)

### Insights:
* The top graph shows specialized technical skills like dplyr, Bitbucket, and Gitlab are associated with higher salaries, some reaching up to $200K, suggesting that advanced technical proficiency can increase earning potential.
* The bottom graph highlights that foundational skills like Excel, PowerPoint, and SQL are the most in-demand, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for employability in data analysis roles.
* There's a clear distinction between the skills that are highest paid and those that are most in-demand. Data analysts aiming to maximize their career potential should consider developing a diverse skill set that includes both high-paying specialized skills and widely demanded foundational skills.

## 4. What are the most optimal skills to learn for Data Analysts?
To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.
View my notebook with detailed steps here: [Optimal Skills](https://github.com/BlagovestNB/Python-Projects/blob/7d5ba7097e118b962d6ade9f646ba0f943797aed/Main_project/4.%20Optimal%20Skills.ipynb)
### Visualize Data

```python
import seaborn as sns
from adjustText import adjust_text

sns.scatterplot(data= merged, x= 'count_percent', y= 'median_salary')
```

### Result
![Alt text](https://github.com/BlagovestNB/Python-Projects/blob/b3f91b9fe124b404eed76678ca5fdb0a1a9c2292/Main_project/Pictures/4.%20Optimal%20Skills.png)

Insights:
* The skill Oracle appears to have the highest median salary of nearly $97K, despite being less common in job postings. This suggests a high value placed on specialized database skills within the data analyst profession.
* More commonly required skills like Excel and SQL have a large presence in job listings but lower median salaries compared to specialized skills like Python and Tableau, which not only have higher salaries but are also moderately prevalent in job listings.
* Skills such as Python, Tableau, and SQL Server are towards the higher end of the salary spectrum while also being fairly common in job listings, indicating that proficiency in these tools can lead to good opportunities in data analytics.

### Visualizing Different Techonologies

Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})

Visualize Data
```python
from matplotlib.ticker import PercentFormatter

sns.scatterplot(
    data= merged,
    x= 'count_percent',
    y= 'median_salary',
    hue='technology',
    palette='bright',
    legend='full'
)

plt.show()
```
Result
![Alt text](https://github.com/BlagovestNB/Python-Projects/blob/b1d60086b01365048e58ec7bf8df16a948977255/Main_project/Pictures/4.1%20Optimal%20Skills.png)

Insights:
* The scatter plot shows that most of the programming skills (colored blue) tend to cluster at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary benefits within the data analytics field.
* The database skills (colored orange), such as Oracle and SQL Server, are associated with some of the highest salaries among data analyst tools. This indicates a significant demand and valuation for data management and manipulation expertise in the industry.
* Analyst tools (colored green), including Tableau and Power BI, are prevalent in job postings and offer competitive salaries, showing that visualization and data analysis software are crucial for current data roles. This category not only has good salaries but is also versatile across different types of data tasks.

## What I Learned
Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:
* Advanced Python Usage: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
* Data Cleaning Importance: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
* Strategic Skill Analysis: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.

### Insights
This project provided several general insights into the data job market for analysts:
* Skill Demand and Salary Correlation: There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.
* Market Trends: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
* Economic Value of Skills: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.

### Challenges I Faced
This project was not without its challenges, but it provided good learning opportunities:
* Data Inconsistencies: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
* Complex Data Visualization: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
* Balancing Breadth and Depth: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.

### Conclusion
This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.
