# Project Overview

This project offers an insightful exploration of the data analytics job market, with a specific focus on Data Analyst roles. Inspired by a personal drive to better understand career opportunities in tech, the analysis investigates the most valuable and in-demand skills to help aspiring or current data analysts navigate the job landscape effectively.

The dataset comes from `Luke Barousse’s Python course`, providing a solid starting point with detailed job listings, salaries, locations, and required skills. Using Python, I analyzed this data to uncover trends and answer pressing questions about demand, salary potential, and skill optimization in the data analytics field.

# Key Questions Explored

In this analysis, I aimed to answer the following questions:

- What are the skills most in demand for the top 3 most popular data roles?

- How are in-demand skills trending for Data Analysts?

- How well do jobs and skills pay for Data Analysts?

- What are the optimal skills for data analysts to learn? (High Demand AND High Paying)

# Tools and Technologies Used

To perform this analysis, I utilized a range of tools and libraries:

Python – The core language for data manipulation and analysis.

Pandas – Used for data wrangling and exploration.

Matplotlib – Helped create foundational data visualizations.

Seaborn – Used to build advanced, more visually appealing charts.

Jupyter Notebooks – Allowed for interactive coding and documentation of findings.

Visual Studio Code – My primary code editor for writing and testing scripts.

Git & GitHub – Used for version control and sharing the project with others.

# Data Cleaning and Preparation

Before diving into analysis, I cleaned and prepared the data to ensure consistency, accuracy, and relevance. This included removing duplicates, handling missing values, parsing complex fields, and restructuring data formats for easier analysis.

# Import & Clean Up Data

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

# Filter US Jobs
To focus my analysis on the U.S. job market, I apply filters to the dataset, narrowing down to roles based in the United States.
```python
df_US = df[df['job_country'] == 'United States']
```


# The Analysis
Each Jupyter Notebook in this project was designed to explore a distinct angle of the data job market. Here's a breakdown of how I tackled each core question through targeted analysis:

## 1. What are the most demanded skills for the top 3 most popular data roles?

To identify the most in-demand skills for the top three most popular data roles, I first filtered the job listings by popularity. Then, for each of these top roles, I extracted the five most frequently mentioned skills. This analysis highlights the key job titles and their associated skill sets, helping me understand which skills to focus on based on the role I aim to pursue.

View my notebook with detailed steps here: [2_skill_demand.ipynb](3_Project\2_skill_demand.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_count[df_skills_count['job_title_short'] == job_title].head(5)
    df_plot.plot(kind='barh', x='job_skills', y='skill_count', ax=ax[i], title=job_title)
    ax[i].invert_yaxis()
    ax[i].set_ylabel('')
    ax[i].legend().set_visible(False)

fig.suptitle('Counts of Top Skills in Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5)
plt.show()
```

### Results

![Visualization of Top Skills for Data Nerds](3_Project\Images\top_skills_for_data_roles.png)
![top_skills_for_data_roles](https://github.com/user-attachments/assets/ee3a1c75-3356-4217-955f-a1c7ec1d7078)



### Insights:

- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%).
- SOL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for both roles. For Deta Engineers, SQL is the most sought-after skill appearing in 68% of 100 postings.
- Data Engineers require more specialized technical skills (AMS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).


# The Analysis
## 2. How are in-demand skills trending for Data Analysts?

### Visualize Data
```python
from matplotlib.ticker import PercentFormatter
sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.set_theme(style='ticks')
sns.despine()

plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('Liklihood in Job posting')
plt.xlabel('2023')
plt.legend().remove()


ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i])

plt.show()
```

### Results

![Trending Top Skills for Data Analysts in the US](3_Project\Images\skill_trend_DA.png)
![skill_trend_DA](https://github.com/user-attachments/assets/65b085f9-a8a5-448d-ae61-dfc1e1ce1469)

*Bar graph visualizing the trending top skills for data analysts in the US in 2023.*

### Insights:
- SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand.
- Excel experienced a significant increase in demand starting around september, surpassing both Python and Tableau by the end of the year.
- Both Python and Tableau show relatively stable demand throughout the year with sone fluctuations but remain essential skills for data analysts. Power BI, while less demanded compared to the others, shows a slight upward trend towards the year's end.


# The Analysis

### 3. How well do jobs and skills pay for Data Analysts?

## Salary Analysis for Data Nerds


### Visualize Data

```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')

plt.title('Salary Distribution in the United States')
plt.xlabel('Yearly Salary ($USD)')
plt.ylabel('')
ax = plt.gca()
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))
plt.xlim(0, 600000)
plt.show()
```

#### Results

![Salary Distributions of Data jobs in the US](3_Project\Images\salary_distribution.png) 
![salary_distribution](https://github.com/user-attachments/assets/50f32433-ccd9-463d-a306-d5df53ceebce)

*Box plot visualizing the salary distributions for the top 6 data job titles.*

#### Insights:

- There is a significant variation in salary ranges across different job titles, Senior Data Scientist positions tend to have the highest salary potential, with up to 600K, Indicating the high value placed on advanced data skills and experience in the industry.

- Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles. In contrast, Data Analyst roles demonstrate more consistency in salary, with fewer outliers.

- The median salaries increase with the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median selaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.


### Highest paid and most demanded skills for Data Analysts
#### Insights:

### Visualize Data

```python
fig, ax = plt.subplots(2, 1)

sns.set_theme(style='ticks')

# Top 10 Highest Paid Skills for Data Analysts

#df_DA_top_pay[::-1].plot(kind='barh', y='median', ax=ax[0], legend=False)
# alternatively, to invert we use ax[0].invert_yaxis()

sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, ax=ax[0], hue='median', palette='dark:b_r')
ax[0].legend().remove()
ax[0].set_title('Top 10 Highest Paid Skills for Data Analysts')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))

# Top 10 Most In-Demand Skills for Data Analysts
#df_DA_skills[::-1].plot(kind='barh', y='median', ax=ax[1], legend=False)

sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, ax=ax[1], hue='median', palette='light:b')
ax[1].legend().remove()
ax[1].set_title('Top 10 Most In-Demand Skills for Data Analysts')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary ($USD)')
ax[1].set_xlim(ax[0].get_xlim())
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))

fig.tight_layout()
```

#### Results
![The Highest Paid & Most In-Demand Skills for Data Analysts in the US](3_Project\Images\top_and_most_in-demand_skills_for_data_analysts.png)
![top_and_most_in-demand_skills_for_data_analysts](https://github.com/user-attachments/assets/504c582e-e2b6-4dd3-8566-f17796782b66)

*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in the US.*

#### The Insights:

- The top graph shows specialized technical skills like `dplyr`, `Bitbucket`, and `Gitlab` are associated with higher salarias, some reaching up to 200k, suggesting that advanced technical proficiency can increase earning potential.

- The botton graph highlights that foundational skills like `Excel`, `PowerPoint`, and `SQL` are the most in-demand, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for employability in data analysis roles.

- There's a clear distinction between the skills that are highest paid and those that are most in-demand. Data analysts aiming to maximize their career potential should consider developing a diverse skill set that includes both high-paying specialized skills and widely demanded foundational skills.

# The Analysis
## 4. What is the most optimal skill to learn in Data analysis?

#### Visualize Data

```python
rom adjustText import adjust_text
import matplotlib.pyplot as plt
import seaborn as sns
from matplotlib.ticker import PercentFormatter

# Plot
sns.set_theme(style='ticks')
sns.scatterplot(
    data=df_plot,
    x='skill_percent',
    y='median_salary',
    hue='technology'  # change/remove if this column doesn't exist
)

sns.despine()

# Label each point
texts = []
for i, row in df_plot.iterrows():
    texts.append(
        plt.text(row['skill_percent'], row['median_salary'], row['skills'])
    )
# Adjust labels
adjust_text(
    texts,
    arrowprops=dict(arrowstyle="->", color='gray', lw=1),
    only_move={'points': 'y', 'text': 'xy'},  # only move in Y direction for better spacing
    expand_text=(1.2, 1.2),
    expand_points=(1.2, 1.2),
    force_text=0.8
)

# Format axes
ax = plt.gca()
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, _: f'${int(y/1000)}K'))

# Labels and title
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Yearly Salary ($USD)')
plt.title('Most Optimal Skills for Data Analysts in the US')

plt.tight_layout()
plt.show()
```

#### Results
[Most Optimal Skills For Data Analysts In The US](3_Project\Images\most_optimal_skill.png)
![most_optimal_skill](https://github.com/user-attachments/assets/44016d9c-5ce3-42cf-84e5-b5207ead8293)

*A scatter plot visualizing the most optimal skills(high paying & high demand) for data analysts in the US.*


#### Insights:
- The scatter plot shows that most of the `programming` skills (colored blue) tend to cluster at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary benefits within the data analytica field.

- Analyst tools (colored green), incluting Tableau and Power BI, are prevalent in job postings and offer competitive salaries, showing that visualization and date analysis software are crucial for current data roles. This category not only has good salaries but is also versatile across different types of data tasks.

- The database skills (colored orange), such as Oracle and SQL Server, are associated with some of the highest salaries among data analyst tools. This indicates a significant demand and valuation for data managment and manipulation expertise in the industry.

#### Visualizations:
![most_optimal_skill](https://github.com/user-attachments/assets/3494151a-ce9d-44e5-914d-7d442f2a39e6)
![salary_distribution](https://github.com/user-attachments/assets/deeb1ff0-01cd-4d9b-a651-0243ab570054)
![skill_trend_DA](https://github.com/user-attachments/assets/f27a281a-0d4e-4fc2-a3b6-3deabb301df1)
![top_and_most_in-demand_skills_for_data_analysts](https://github.com/user-attachments/assets/b9da8f7e-5001-4658-81b8-3f97303e705f)
![top_skills_for_data_roles](https://github.com/user-attachments/assets/75c0523d-2397-4061-bdd5-4f02216ac23b)






# What I Learnt from the Project
This project offered valuable lessons, both in understanding the data analyst job landscape and in strengthening my Python skills—particularly in working with data and creating insightful visuals. Here are some specific takeaways:

Mastering Python Libraries: I significantly improved my use of essential Python libraries. Pandas helped me efficiently transform data, while Seaborn and Matplotlib allowed me to create compelling visual narratives. These tools made it easier to tackle advanced analysis tasks.

Crucial Role of Data Cleaning: I came to fully appreciate how essential it is to clean and structure data before analysis. Clean data forms the foundation for generating meaningful and accurate insights.

Strategic Skill Positioning: I learned that understanding which skills are in high demand—and how they relate to compensation and job availability—is a major advantage. This kind of insight can help steer career decisions more strategically in the tech world.

Key Insights
Through this analysis, several important patterns about the data analyst job market became clear:

High-Demand Skills Drive Pay: There’s a noticeable connection between how sought-after a skill is and how well it pays. For example, tools like Python and Oracle are not only widely required but also associated with higher salaries.

Shifting Industry Needs: Skill demands evolve over time, reinforcing the need for continuous learning. Adapting to these shifts is critical for staying competitive.

Economic Impact of Skillsets: Identifying which skills are both valuable and scarce can help data professionals focus their learning efforts for the best career returns.

# Challenges Encountered
While the project was enriching, it came with its fair share of hurdles—each offering an opportunity to grow:

Inconsistent Data Quality: Dealing with incomplete or messy data forced me to apply careful data-wrangling methods to maintain accuracy and trustworthiness in the final analysis.

Visualizing Complex Data: Turning layered data into visuals that are both informative and easy to understand proved challenging, yet essential for effective communication.

Finding the Right Scope: Striking a balance between detailed deep-dives and maintaining a high-level perspective was tricky. It required conscious effort to avoid losing sight of the bigger picture.

# Conclusion
This deep dive into the data analyst job market was both eye-opening and instructive. It clarified the skills that matter most and shed light on current industry trends. The findings not only strengthened my data analysis capabilities but also laid a foundation for ongoing research. As the data landscape evolves, staying informed and adaptable will be key—and this project marks just the beginning of that journey.
