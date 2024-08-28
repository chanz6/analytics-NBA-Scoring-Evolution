# NBA Scoring Evolution Analysis
## Introduction
It is well known that NBA players are constantly improving. The question begs, how rapidly are they improving? What exactly is causing the NBA's rapidly improving offensive efficiency?
This is a short project where I answer those two questions. This project utilizes Pandas and Numpy to view, clean and manipulate the data, as well as matplotlib to visualize results.  

The dataset I used was scraped from the NBA's official stats page: [https://www.nba.com/stats/leaders](url). The scraping process is documented in the **stats scraping.ipynb** file.
I also saved the full dataset as an Excel file, **nba_player_data.xlsx**. 
This dataset includes the total stats for each player from the 2012-13 season through the 2021-22 season, with individual stats recorded seperately for each season.

## Cleaning the Data
![](images/capture1.PNG)
![](images/capture2.PNG)
### First Inspection
Upon first inspection of the data, we can observe that it is already fairly clean. 
It seems there are no non-null values, and most, if not all, columns are of the correct type.
However, there are a couple of minor changes we can make to make the analysis easier:  
1. Drop the 'RANK' (Player Ranking) and 'EFF' (Efficiency) columns, as we will not need them for our analysis.
2. Add a column with the starting year of each season, to make the 'Year' (season) column easier to reference.
3. In the 'TEAM' column, merge 'NOH' and 'NOP' into one team (since New Orleans changed their team name).
4. In the 'Season_type' column, rename 'Regular%20Season' to a more readable format.      

![](images/capture3.PNG)
![](images/capture4.PNG)
![](images/capture5.PNG)
![](images/capture6.PNG)

### Organizing the Data
Now that our data is ready for analysis, we can organize it further to ensure more efficient analysis:
- Seperate the dataframes by regular season and playoffs.
- Add column names that are suitable for future aggregation to an array for easy reference.

![](images/capture7.PNG)
![](images/capture8.PNG)

## Analysis
We begin by calculating the following stats for each individual season, which are each stored in seperate dictionaries:
- Total FGM (field goals made)
- Total 3PM (three pointers made)
- Total 3PA (three pointers attempted)
- Total points scored

**Note**: Keys represent the year the season started

![](images/capture9.PNG)
![](images/capture10.PNG)
![](images/capture11.PNG)
![](images/capture12.PNG)

We then create a dataframe with our dictionaries, which will allow us to plot the data more efficiently.
By combining the per-season stats into a single DataFrame, we streamline the analysis process, making it easier to see trends and patterns across seasons.  

The DataFrame includes the following columns:  
- **'season'**: The NBA season
- **'season_start_year'**: The year the season started
- **'FGM'**: Total field goals made for that season
- **'3PA'**: Total three-pointers attempted for that season
- **'3PM'**: Total three-pointers made for that season
- **'PTS'**: Total points scored for that season

![](images/capture13.PNG)

### Inspection
By examining our new DataFrame, we can already begin to make some key observations:
1. Total FGM steadily increases, but it suddenly plummets during the 2019-20 and 2020-21 seasons.
2. 3PA increases rapidly, but it too declines during the 2019-20 and 2020-21 seasons.
3. 3PM & PTS are increasing but also decline during the same period.

These declines during the 2019-20 and 2020-21 seasons are likely due to covid, which shortened both of those seasons.
To maintain accuracy and consistency of our analysis, we will remove these rows from the DataFrame.

![](images/capture14.PNG)

Finally, we calculate the total 3P% (three point field goal percentage) for each season. We also create a DataFrame that tracks the change in FGM, 3PA, 3PM, and PTS from the previous season. This will be used later to calculate the maximum and average increase in these stats.

![](images/capture15.PNG)
![](images/capture16.PNG)

### Points Scored
The graph below shows the total points scored across different seasons. We can observe that the total points have been rising steadily. The largest increase between two seasons was 3645 points, with an average increase of 4744 points per season.

![](images/capture17.PNG)
![](images/capture18.PNG)

### Field Goals Made
The graph below illustrates the number of field goals made (FGM) across various seasons. We can see a consistent increase in FGM over the seasons. The largest increase between two seasons was 3645 field goals, with an average increase of approximately 1333 field goals per season.

![](images/capture19.PNG)
![](images/capture20.PNG)

### Three Pointers Made
The graph below shows the number of three-pointers made (3PM) across different seasons. The steady rise in 3PM suggests a growing reliance on three-point shots. The largest increase between two seasons was 2916 three-pointers, with an average increase of nearly 1989 three-pointers per season. This increase in three-point shooting may be a significant factor contributing to the overall rise in total points scored.

![](images/capture21.PNG)
![](images/capture22.PNG)

### Three Pointers Attempted
The graph depicting three-pointers attempted (3PA) follows a similar upward trend, reinforcing the shift toward three-point shooting in recent seasons. The largest increase between two seasons was 8409 attempts, with an average increase of approximately 5712 attempts per season. The strong correlation between the rise in 3PA and 3PM underscores the growing importance of the three-point shot in today's game, likely driving the overall increase in scoring.

![](images/capture23.PNG)
![](images/capture24.PNG)

### Relationship Between 3PM and 3PA
The scatter plot of 3PM vs. 3PA further demonstrates the strong positive correlation between the number of three-pointers attempted and those made. As teams increasingly rely on three-point shots, the data shows that a higher volume of attempts generally leads to more made three-pointers, which is a key driver of the rise in scoring across seasons.

![](images/capture25.PNG)

### Relationship Between Total Points (PTS) and 3PM
Finally, the scatter plot of total points scored (PTS) versus three-pointers made (3PM) shows a clear relationship between these two variables. As teams score more three-pointers, the overall points scored in a season tend to increase. This reinforces the idea that the rise in three-point shooting is a huge factor contributing to the overall increase in scoring across seasons.

![](images/capture26.PNG)

### Further Analysis: Three Point Percentage (3P%)
The graph below of 3-point percentage (3P%) over the seasons shows some ups and downs instead of a steady increase. Even though teams are shooting more three-pointers and making more of them, their accuracy hasn't consistently improved. This means that while teams are taking more shots from beyond the arc, they aren't necessarily getting better at making them.

![](images/capture27.PNG)

The scatter plot below comparing 3-point percentage (3P%) with total points scored (PTS) shows that there isn't a strong relationship between the two. This indicates that the accuracy of three-point shooting hasn't been a key factor in the overall rise of points scored across seasons. Instead, the increase in total points seems to be driven more by the sheer volume of three-point attempts and makes, rather than improvements in shooting efficiency.

![](images/capture28.PNG)

## Conclusion
The data shows that the NBA’s offensive efficiency has been rapidly improving, but this improvement isn't primarily due to better shooting accuracy. Instead, the main driver is the significant increase in three-point attempts. Teams are taking far more shots from beyond the arc, which leads to more three-pointers being made, even if the shooting percentage hasn’t consistently improved. This strategic shift toward a higher volume of three-point shots is the key factor behind the rise in total points scored and the overall boost in offensive efficiency across the league.
