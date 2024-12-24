# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

View my notebook with detailed steps here: [2_Skills_Count.ipynb](2_Project/2_Skills_Count.ipynb)


### Visualize Data

```python
# plotting 3 rows 1 column

fig, ax = plt.subplots(len(job_titles), 1)

sns.set_theme(style='ticks')

for i, job_title in enumerate(job_titles):

    # find jobs that are on the job_titles list
    df_plot = df_skills_percent[df_skills_percent['job_title_short'] == job_title].head(5)

    # plot chart using seaborn
    sns.barplot(
        data=df_plot,
        x='skill_percent',
        y='job_skills',
        ax=ax[i],
        hue='skill_count',
        palette='dark:b_r'
    )
    ax[i].set_title(job_title)
    ax[i].set_xlabel('')
    ax[i].set_ylabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0,78)

    # add labels to each bar by looping through df_plot
    for index, value in enumerate(df_plot['skill_percent']):
        ax[i].text(value +1 , index, f'{value:.0f}%', va='center') 
        # value +1 so its a bit far away from the bar
        # va='center' to position labels in the center
        # f'{value:.0f}%' converts the numbers to a float with no decimal places
    # remove xaxis and only keeping the bottom one
    if i != len(job_titles) -1:
        ax[i].set_xticks([])

        
fig.suptitle('Likelihood of Skills Requested in US Job POstings', fontsize=15)
fig.tight_layout(h_pad=0.5) # fix overlap
plt.show()
```
### Results
![image_skill_demand_all_roles](/2_Project/images/skill_demand_all_data_roles.png)  

### Insights


# The Analysis

## 2. How are in-demand skills trending for Data Analysts?
### Visualize Data
```python
df_plot = df_DA_US_percent.iloc[:,:5]

sns.lineplot(
    data=df_plot,
    dashes=False,
    palette='tab10'
)
sns.set_theme(style='ticks')
sns.despine()

plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()

# change yaxis to percentage
from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

# use for loop to add label to each line
for i in range (5):
    plt.text(11.2,df_plot.iloc[-1,i],df_plot.columns[i])
```

### Results
![Skills_trend](/2_Project/images/top_skill_trends.png)  
*Line chart visualizing the trending top skills for data analysts in the US in 2023.*

### Insights

# The Analysis

## 3. How well do jobs and skills pay for Data Analysts?

### Salary Analysis

### Visualize Data
```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')

plt.title('Salary Distribution in the US')
plt.xlabel('Yearly Salary ($USD)')
plt.ylabel('')
plt.xlim(0,600000)
ticks_x=plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

### Results
![salary_analysis](/2_Project/images/salary_distribution.png)  
*Box plot visualizing the salary distribution for the top 6 data jobs.*

### Insights

# The Analysis

## 3. How well do jobs and skills pay for Data Analysts?
### Highest Paid & Most Demanded Skills for Data Analysts
#### Visualize Data
```python
fig, ax = plt.subplots(2,1)

#set seaborn theme
sns.set_theme(style='ticks')
# plot df_DA_top_pay
# use seaborn.barplot
sns.barplot(data=df_DA_top_pay,x='median',y=df_DA_top_pay.index,ax=ax[0],hue ='median',palette='dark:b_r') # '_r' to reverse colroing
ax[0].legend().remove() # remove legend
ax[0].set_title('Top 10 Highest Paid Skills for Data Analysts')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x,_: f'${int(x/1000)}K'))
# plot df_DA_skills
# use seaborn.barplot
sns.barplot(
    data=df_DA_skills,
    x='median',
    y=df_DA_skills.index,
    ax=ax[1],
    hue ='median',
    palette='light:b'
)
ax[1].legend().remove() # remove legend
ax[1].set_title('Top 10 Most In-Demand Skills for Data Analysts')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary ($USD)')
ax[1].set_xlim(ax[0].get_xlim())
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x,_: f'${int(x/1000)}K'))
plt.tight_layout()
plt.show()
```

### Results
![top_paid_and_top_demand_skills](/2_Project/images/top_paid_and_top_demand_skills.png)  
*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in the US.*  

### Insights

# The Analysis

## 4. What is the most optimal skill to learn for Data Analysts?
#### Visualize Data
```python
from adjustText import adjust_text
from matplotlib.ticker import PercentFormatter
df_DA_skills_high_demand.plot(kind='scatter',x='skill_percent',y='median_salary')
# empty list to collect x and y values and label names in the for loop below
texts = []
# create a loop to find skill location (x,y)
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i],df_DA_skills_high_demand['median_salary'].iloc[i],txt))
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray', lw=1))
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))
# set axis labels, title and legend
plt.xlabel('Percent of Data Analyst Job Postings')
plt.ylabel('Median Yearly Salary ($USD)')
plt.title('Most Optimal Skills for Data Analysts in the US')
plt.tight_layout()
plt.show()
```
### Results
![optimal_skills](/2_Project/images/optimal_skills.png)  
*A scatter plot visualizing the most optimal skills (high paying and high demand) for data analysts in the US.*  

### Insights
