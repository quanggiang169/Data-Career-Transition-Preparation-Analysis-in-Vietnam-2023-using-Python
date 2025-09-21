# Project Overview
This project explores the Vietnamese job market for Data Analyst and related roles using real-world job posting data. The analysis is designed for professionals considering a career transition into data analytics, with the goal of answering two big questions:
- What does the current job market for Data Analysts in Vietnam look like?
- How can career changers strategically prepare themselves to succeed in this field?

# The Analysis
## 1. Which skills are most effective for newcomers (in terms of opportunities & salary)?
I analyzed skills by grouping them to calculate median salaries and their likelihood of appearing in job postings. The results were visualized to show the relationship between salary potential and demand across skills. This highlighted which technologies are both more prevalent and more rewarding for newcomer

View my notebook with detailed steps here:
[2. High_ROI_Skills.ipynb](2.%20High_ROI_Skills.ipynb)

#### Visualize Data
```python
# Scatter plot: likelihood vs salary, highlight technologies
sns.scatterplot(data=df_plot, x='skil_likelihood', y='median_salary', hue='technology')

# Add & adjust text labels to avoid overlap
from adjustText import adjust_text
texts = [plt.text(x, y, txt) for x, y, txt in zip(df['skil_likelihood'], df['median_salary'], df.index)]
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray', lw=0.5))

# Format axis for readability
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, _: f'{int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))
```

#### Result
![Which skills are most effective for newcomers (in terms of opportunities & salary)?](images/most_optimal_skills_da.png)

#### Insights
- **SQL** stands out as the most effective skill, offering both the highest demand (appearing in ~70% of postings) and one of the strongest salary levels, making it essential for newcomers.
- **Python** also provides strong opportunities, balancing relatively high demand (~35%) with one of the top salary ranges, showing its value for career growth.
- While **Power BI** are widely requested, they are associated with lower salary levels compared to programming and database skills, suggesting they are useful complements but not the primary drivers of higher pay.

## 2. How can Data Analyst maximize salary — what to learn and where to work in Vietnam?

- 📍 Location matters: Hanoi tends to reward analysts with higher pay, while Ho Chi Minh City offers more job openings and stronger demand.
- 💻 Skills that pay: Focus on SQL and Python — they’re the backbone of most roles and give you both stability and growth. Niche tools can add value, but they shouldn’t replace your core stack.
- 🎓 Degree not required: Many high-paying opportunities don’t demand a formal degree, proving that practical skills and adaptability can take you just as far — especially in remote roles.

View my notebook with detailed steps here:
[3. Salary_Maximization_Strategies.ipynb](3.%20Salary_Maximization_Strategies.ipynb)

### 2.1. Which locations offer the best balance of salary and job demand?
I analyzed job postings by grouping them based on location to calculate the median salary for each city. The results were visualized to compare demand, measured by posting counts, against pay, represented by median salary. This highlighted which locations offer both strong hiring activity and attractive compensation levels

#### Visualization
```python
# Scatter plot: job demand vs median salary by location
plt.scatter(df_plot['no_job_posted'], df_plot['median_salary'], s=200, color='royalblue')

# Add city labels
for city, row in df_plot.iterrows():
    plt.text(row['no_job_posted']+1, row['median_salary'], city, fontsize=9)

# Format salary axis in $K
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda x,_: f'${int(x/1000)}K'))
```
#### Result
![Which locations offer the best balance of salary and job demand?](images/locations_offer_best_salary_job_demand.png)

#### Insights
- **Hanoi** offers the highest median salary (≈$111K), though with fewer postings, making it attractive for those prioritizing pay.
- **Ho Chi Minh City** provides more job opportunities (≈120 postings) but with slightly lower salaries (≈$101K), suggesting stronger demand but less pay leverage.

### 2.2.  Which technical skills provide the best balance between salary potential and market demand?
I analyzed technical skills by grouping them to calculate both median salaries and demand levels across postings. The analysis identified the top 10 skills in terms of salary and the top 10 in terms of demand, allowing a direct comparison between earning potential and market need. The results were visualized with bar charts to highlight which skills strike the best balance between strong pay and widespread relevance.

#### Visualization
```python
# Compare top-paying vs most in-demand skills for Data Analysts
fig, ax = plt.subplots(2,1, figsize=(8,6))

sns.barplot(data=df_da_top_pay, x='median', y='job_skills', ax=ax[0], palette='dark:b_r')
ax[0].set_title('Top 10 Highest Paid Skills')

sns.barplot(data=df_da_skills, x='median', y='job_skills', ax=ax[1], palette='light:b')
ax[1].set_title('Top 10 Most In-Demand Skills')

# Format salary axis in $K
for a in ax:
    a.xaxis.set_major_formatter(plt.FuncFormatter(lambda x,_: f'${int(x)/1000}K'))
```
#### Result
![Which technical skills provide the best balance between salary potential and market demand?](images/top_10_highest_paid_skills.png)

#### Insights
- **SQL** and **Python** stand out as the most effective skills, appearing at the top of demand rankings while also providing strong salaries, making them essential for career stability and growth.
- High-paying but less common skills like **Looker** and **Word** offer strong salary potential, but their limited demand suggests they are niche rather than core skills for most data analyst roles.

### 2.3. Can newcomers still earn competitive salaries without a degree or with remote roles?
I analyzed job postings by splitting them based on degree requirements and remote availability. Median salaries were then compared across these groups to understand the trade-off between accessibility and pay. Skill requirements were also overlaid to identify which accessible roles still provide strong earning potential, with results visualized through boxplots.

#### Visualization
```python
# Median salary by accessibility (Degree/No degree, Remote/On-site)
df_grouped = df_plot.groupby(
    df_plot.apply(lambda r: f"{'No degree' if r.job_no_degree_mention else 'Degree'} | "
                            f"{'Remote' if r.job_work_from_home else 'On-site'}", axis=1)
).agg(median_salary=('median_salary','median'), n=('no_job_posted','sum'))

sns.barplot(data=df_grouped, x=df_grouped.index, y='median_salary', palette=palette)
for p, n in zip(ax.patches, df_grouped['n']):
    ax.text(p.get_x()+p.get_width()/2, p.get_height()+1000, f"n={n}", ha='center')
```

#### Result
![Can newcomers still earn competitive salaries without a degree or with remote roles?](images/competitive_salaries_without_degree.png)

#### Insights
- Roles that **do not require a degree** offer significantly higher median salaries (≈$107K) compared to traditional degree-required positions (≈$72K), showing strong earning potential even without formal education.
- Although degree-required jobs are slightly more common (164 postings vs. 151), the salary premium in no-degree roles suggests newcomers can still compete effectively without academic credentials.

## 3. How do DA opportunities compare with other roles, and how transferable are DA skills?

View my notebook with detailed steps here:
[4. Career_Pathways_Beyond_DA.ipynb](4.20%Career_Pathways_Beyond_DA.ipynb)

### 3.1.

#### Visualization

#### Result

#### Insights

### 3.2.

#### Visualization

#### Result

#### Insights

### 3.3.

#### Visualization

#### Result

#### Insights