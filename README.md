# NBA Scoring Evolution Analysis
## Introduction
It is well known that NBA players are constantly improving. The question begs, how rapidly are they improving? What exactly is causing the NBA's rapidly improving offensive efficiency?
This is a short project where I answer those two questions. This project utilizes Pandas and Numpy to view, clean and manipulate the data, as well as matplotlib and seaborn to visualize results.  

The dataset I used was scraped from the NBA's official stats page: [https://www.nba.com/stats/leaders](url). The scraping process is documented in the ```stats scraping.ipynb``` file.
I also saved the full dataset as an Excel file, ```nba_player_data.xlsx```. 
This dataset includes the total stats for each player from the 2012-13 season through the 2021-22 season, with individual stats recorded seperately for each season.

## Cleaning the Data
```
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt

data = pd.read_excel('nba_player_data.xlsx')
```
```
data.sample(10)
```
![](images/capture1.PNG)
```
data.shape
```
![](images/capture2.PNG)
```
data.info()
```
![](images/capture3.PNG)

### First Inspection
Upon first inspection of the data, we can observe that it is already fairly clean. 
It seems there are no non-null values, and most, if not all, columns are of the correct type.
However, there are a couple of minor changes we can make to make the analysis easier:  
1. Drop the 'RANK' (Player Ranking) and 'EFF' (Efficiency) columns, as we will not need them for our analysis.
2. Add a column with the starting year of each season, to make the 'Year' (season) column easier to reference.
3. In the 'TEAM' column, merge 'NOH' and 'NOP' into one team (since New Orleans changed their team name).
4. In the 'Season_type' column, rename 'Regular%20Season' to a more readable format.      
```
#Drop 'RANK' & 'EFF' columns
data.drop(columns=['RANK','EFF'], inplace=True)
```
```
#Create the season start year column
data['season_start_year'] = data['Year'].str[:4].astype(int)
```
```
data.TEAM.unique()
```
![](images/capture4.PNG)
```
#Change 'NOP' & 'NOH' to represent one team
data['TEAM'].replace(to_replace=['NOP','NOH'], value='NO', inplace=True)
```
```
#Fix regular season
data['Season_type'].replace('Regular%20Season','RS', inplace=True)
```

### Organizing the Data
Now that our data is ready for analysis, we can organize it further to ensure more efficient analysis:
- Seperate the dataframes by regular season and playoffs.
- Add column names that are suitable for future aggregation to an array for easy reference.
```
#Seperate dataframes by regular season and playoffs
rs_df = data[data['Season_type']=='RS']
playoffs_df = data[data['Season_type']=='Playoffs']
```
```
data.columns
```
![](images/capture5.PNG)
```
#Array of columns that makes sense to aggregate
total_cols = ['MIN','FGM','FGA','FG3M','FG3A','FTM','FTA','OREB','DREB','REB','AST','STL','BLK','TOV','PF','PTS']
```
## Analysis
We begin by calculating the following stats for each individual season, which are each stored in seperate dictionaries:
- Total FGM (field goals made)
- Total 3PM (three pointers made)
- Total 3PA (three pointers attempted)
- Total points scored

**Note**: Keys represent the year the season started
```
#Calculate total FGM per season
fgm_per_season = {}
for i in range(min(data['season_start_year']), max(data['season_start_year']) +1):
    fgm_sum = int(data[data['season_start_year'] == i]['FGM'].sum())
    fgm_per_season[i] = fgm_sum
fgm_per_season
```
![](images/capture7.PNG)
```
#Calculate total 3PM per season
threes_per_season = {}
for i in range(min(data['season_start_year']), max(data['season_start_year']) +1):
    threes_sum = int(data[data['season_start_year'] == i]['FG3M'].sum())
    threes_per_season[i] = threes_sum
threes_per_season
```
![](images/capture8.PNG)
```
#Calculate total 3PA per season
threes_attempted_per_season = {}
for i in range(min(data['season_start_year']), max(data['season_start_year']) +1):
    threes_attempted_sum = int(data[data['season_start_year'] == i]['FG3A'].sum())
    threes_attempted_per_season[i] = threes_attempted_sum
threes_attempted_per_season
```
![](images/capture9.PNG)
```
#Calculate total points per season
pts_per_season = {}
for i in range(min(data['season_start_year']), max(data['season_start_year']) +1):
    pts_sum = int(data[data['season_start_year'] == i]['PTS'].sum())
    pts_per_season[i] = pts_sum
pts_per_season
```
![](images/capture10.PNG)

We then create a dataframe with our dictionaries, which will allow us to plot the data more efficiently.
By combining the per-season stats into a single DataFrame, we streamline the analysis process, making it easier to see trends and patterns across seasons.  

The DataFrame includes the following columns:  
- **'season'**: The NBA season
- **'season_start_year'**: The year the season started
- **'FGM'**: Total field goals made for that season
- **'3PA'**: Total three-pointers attempted for that season
- **'3PM'**: Total three-pointers made for that season
- **'PTS'**: Total points scored for that season

```
#Put all per season stats into a single dataframe
stats_df = pd.DataFrame(columns = ['season', 'season_start_year', 'FGM', '3PA', '3PM', 'PTS'])
stats_df['season'] = data['Year'].unique()
stats_df['season_start_year'] = fgm_per_season.keys()
stats_df['FGM'] = fgm_per_season.values()
stats_df['3PA'] = threes_attempted_per_season.values()
stats_df['3PM'] = threes_per_season.values()
stats_df['PTS'] = pts_per_season.values()
stats_df
```
![](images/capture11.PNG)

### Inspection
By examining our new DataFrame, we can already begin to make some key observations:
1. Total FGM steadily increases, but it suddenly plummets during the 2019-20 and 2020-21 seasons.
2. 3PA increases rapidly, but it too declines during the 2019-20 and 2020-21 seasons.
3. 3PM & PTS are increasing but also decline during the same period.

These declines during the 2019-20 and 2020-21 seasons are likely due to covid, which shortened both of those seasons.
To maintain accuracy and consistency of our analysis, we will remove these rows from the DataFrame.
```
#Remove 2019-2020 and 2020-2021 seasons
stats_df = stats_df[(stats_df['season_start_year'] != 2019) & (stats_df['season_start_year'] != 2020)]
```

Finally, we calculate the total 3P% (three point field goal percentage) for each season. We also create a DataFrame that tracks the change in FGM, 3PA, 3PM, and PTS from the previous season. This will be used later to calculate the maximum and average increase in these stats.
```
#Calculate 3P% by season
stats_df['3P%'] = stats_df['3PM'] / stats_df['3PA']
stats_df['3P%']
```
![](images/capture12.PNG)
```
#Create a dataframe for the change in FGM, 3PA, 3PM, PTS from the previous season
diff = stats_df[['FGM', '3PA', '3PM', 'PTS']].diff()
```

### Points Scored
The graph below shows the total points scored across different seasons. We can observe that the total points have been rising steadily. The largest increase between two seasons was 12,527 points, with an average increase of 4,744 points per season.
```
#Import seaborn
import seaborn as sns
sns.set_theme()

#Set Seaborn Style
sns.set(style='whitegrid')

#Plot PTS by season
plt.figure(figsize=(14,7))
sns.lineplot(x=stats_df['season'], y=stats_df['PTS'], marker='o')
plt.xticks(stats_df['season'], rotation=45)
plt.title('PTS by Season', fontsize=16, fontweight='bold')
plt.xlabel('Season')
plt.ylabel('Points (PTS)')
plt.show()
```
![](images/capture13.PNG)
```
#Display max increase in pts and avg increase in pts 
pts_max_diff = np.nanmax(diff['PTS'])
pts_avg_increase = np.nanmean(diff['PTS'])
print('Maximum Increase: ' + str(pts_max_diff) + ' points, Average Increase: ' + str(pts_avg_increase) + ' points') 
```
![](images/capture21.PNG)
### Field Goals Made
The graph below illustrates the number of field goals made (FGM) across various seasons. We can see a consistent increase in FGM over the seasons. The largest increase between two seasons was 3,645 field goals, with an average increase of approximately 1,333 field goals per season.
```
#Display FGM by season
plt.figure(figsize=(14,7))
sns.lineplot(x=stats_df['season'], y=stats_df['FGM'], marker='o')
plt.xticks(stats_df['season'], rotation=45)
plt.title('FGM by Season', fontsize=16, fontweight='bold')
plt.xlabel('Season')
plt.ylabel('Field Goals Made (FGM)')
plt.show()
```
![](images/capture14.PNG)
```
#Display max increase in FGM and avg increase in FGM 
fgm_max_diff = np.nanmax(diff['FGM'])
fgm_avg_increase = np.nanmean(diff['FGM'])
print('Maximum Increase: ' + str(fgm_max_diff) + ' field goals, Average Increase: ' + str(fgm_avg_increase) + ' field goals') 
```
![](images/capture22.PNG)
### Three Pointers Made
The graph below shows the number of three-pointers made (3PM) across different seasons. The steady rise in 3PM suggests a growing reliance on three-point shots. The largest increase between two seasons was 2,916 three-pointers, with an average increase of nearly 1,989 three-pointers per season. This increase in three-point shooting may be a significant factor contributing to the overall rise in total points scored.
```
#Plot 3PM by season
plt.figure(figsize=(14,7))
sns.lineplot(x=stats_df['season'], y=stats_df['3PM'], marker='o')
plt.xticks(stats_df['season'], rotation=45)
plt.title('3PM by Season', fontsize=16, fontweight='bold')
plt.xlabel('Season')
plt.ylabel('3-Pointers Made (3PM)')
plt.show()
```
![](images/capture15.PNG)
```
#Display max increase in 3PM and avg increase in 3PM
threes_max_diff = np.nanmax(diff['3PM'])
threes_avg_increase = np.nanmean(diff['3PM'])
print('Maximum Increase: ' + str(threes_max_diff) + ' 3 pointers made, Average Increase: ' + str(threes_avg_increase) + ' 3 pointers made') 
```
![](images/capture23.PNG)
### Three Pointers Attempted
The graph depicting three-pointers attempted (3PA) follows a similar upward trend, reinforcing the shift toward three-point shooting in recent seasons. The largest increase between two seasons was 8,409 attempts, with an average increase of approximately 5,712 attempts per season. The strong correlation between the rise in 3PA and 3PM underscores the growing importance of the three-point shot in today's game, likely driving the overall increase in scoring.
```
#Plot 3PA by season
plt.figure(figsize=(14,7))
sns.lineplot(x=stats_df['season'], y=stats_df['3PA'], marker='o')
plt.xticks(stats_df['season'], rotation=45)
plt.title('3PA by Season', fontsize=16, fontweight='bold')
plt.xlabel('Season')
plt.ylabel('3-Pointers Attempted (3PA)')
plt.show()
```
![](images/capture16.PNG)
```
#Display max increase in 3PA and avg increase in 3PA
threes_attempted_max_diff = np.nanmax(diff['3PA'])
threes_attempted_avg_increase = np.nanmean(diff['3PA'])
print('Maximum Increase: ' + str(threes_attempted_max_diff) + ' 3s attempted, Average Increase: ' + str(threes_attempted_avg_increase) + ' 3s attempted') 
```
![](images/capture24.PNG)
### Relationship Between 3PM and 3PA
The scatter plot of 3PM vs. 3PA further demonstrates the strong positive correlation between the number of three-pointers attempted and those made. As teams increasingly rely on three-point shots, the data shows that a higher volume of attempts generally leads to more made three-pointers, which is a key driver of the rise in scoring across seasons.
```
#Display scatter plot of 3PM vs 3PA
plt.figure(figsize=(14,7))
sns.scatterplot(x=stats_df['3PM'], y=stats_df['3PA'])
plt.title('3PM vs 3PA', fontsize=16, fontweight='bold')
plt.xlabel('3-Pointers Made (3PM)')
plt.ylabel('3-Pointers Attempted (3PA)')
plt.show()
```
![](images/capture17.PNG)
### Relationship Between Total Points (PTS) and 3PM
Finally, the scatter plot of total points scored (PTS) versus three-pointers made (3PM) shows a clear relationship between these two variables. As teams score more three-pointers, the overall points scored in a season tend to increase. This reinforces the idea that the rise in three-point shooting is a huge factor contributing to the overall increase in scoring across seasons.
```
#Display scatter plot of PTS vs 3PM
plt.figure(figsize=(14,7))
sns.scatterplot(x=stats_df['PTS'], y=stats_df['3PM'])
plt.title('PTS vs 3PM', fontsize=16, fontweight='bold')
plt.xlabel('Points (PTS)')
plt.ylabel('3-Pointers Made (3PM)')
plt.show()
```
![](images/capture18.PNG)
### Further Analysis: Three Point Percentage (3P%)
The graph below of 3-point percentage (3P%) over the seasons shows some ups and downs instead of a steady increase. Even though teams are shooting more three-pointers and making more of them, their accuracy hasn't consistently improved. This means that while teams are taking more shots from beyond the arc, they aren't necessarily getting better at making them.
```
#Plot 3P% by season
plt.figure(figsize=(14,7))
sns.lineplot(x=stats_df['season'], y=stats_df['3P%'], marker='o')
plt.xticks(stats_df['season'], rotation=45)
plt.title('3P% by Season', fontsize=16, fontweight='bold')
plt.xlabel('Season')
plt.ylabel('3-Point Percentage (3P%)')
plt.show()
```
![](images/capture19.PNG)
The scatter plot below comparing 3-point percentage (3P%) with total points scored (PTS) shows that there isn't a strong relationship between the two. This indicates that the accuracy of three-point shooting hasn't been a key factor in the overall rise of points scored across seasons. Instead, the increase in total points seems to be driven more by the sheer volume of three-point attempts and makes, rather than improvements in shooting efficiency.
```
#Display scatter plot of 3P% vs PTS
plt.figure(figsize=(14,7))
sns.scatterplot(x=stats_df['3P%'], y=stats_df['PTS'])
plt.title('3P% vs PTS', fontsize=16, fontweight='bold')
plt.xlabel('3-Point Percentage (3P%)')
plt.ylabel('Points (PTS)')
plt.show()
```
![](images/capture20.PNG)
## Conclusion
The data shows that the NBA’s offensive efficiency has been rapidly improving, but this improvement isn't primarily due to better shooting accuracy. Instead, the main driver is the significant increase in three-point attempts. Teams are taking far more shots from beyond the arc, which leads to more three-pointers being made, even if the shooting percentage hasn’t consistently improved. This strategic shift toward a higher volume of three-point shots is the key factor behind the rise in total points scored and the overall boost in offensive efficiency across the league.
