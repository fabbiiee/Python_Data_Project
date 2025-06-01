# The Analysis

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


### Insights

-Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%).
-SOL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for both roles. For Deta Engineers, SQL is the most sought-after skill appearing in 68% of 100 postings.
-Data Engineers require more specialized technical skills (AMS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).