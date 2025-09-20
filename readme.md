# Project Overview
This project explores the Vietnamese job market for Data Analyst and related roles using real-world job posting data. The analysis is designed for professionals considering a career transition into data analytics, with the goal of answering two big questions:
- What does the current job market for Data Analysts in Vietnam look like?
- How can career changers strategically prepare themselves to succeed in this field?

# The Analysis
## 1. Which skills are most effective for newcomers (in terms of opportunities & salary)?
I analyzed skills by grouping them to calculate median salaries and their likelihood of appearing in job postings. The results were visualized to show the relationship between salary potential and demand across skills. This highlighted which technologies are both more prevalent and more rewarding for newcomer

View my notebook with detailed steps here:
[2. High_ROI_Skills.ipynb](2.%20High_ROI_Skills.ipynb)

### Visualize Data
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

### Result
![Which skills are most effective for newcomers (in terms of opportunities & salary)?](images\most_optimal_skills_da.png)

### Insights

