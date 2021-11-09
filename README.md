# Juno-Data-Analytics Project 1

For this project, we need to determine the retention rate of a mobile games' players based on whether or not they played a match at least 30 days after joining the platform. In order to do so, we need to find the last date each unique player competed in a match and compare that to the day they joined the platform. These two pieces of information are found in different tables in our dataset, so our SQL Query must join the data together before we can proceed. Using player_id as the common piece of information to link the table containing match data with the table containing player data, we begin constructing our query. When selecting the match date data, we're looking for the latest match they've played so we use the MAX function in SQL to only draw the highest date available in the data set. This also necessiates adding the Group By syntax to ensure that the data we're pulling into our final table is organized. For further analysis, I've also included the location, age and system information in my query. 

```
SELECT
  m.player_id,
  MAX(day) AS day,
  joined,
  location,
  age,
  system
FROM
  `juno-college.GameCompanyDataset.matches_info` m
JOIN
  `juno-college.GameCompanyDataset.player_info` p
ON
  m.player_id = p.player_id
GROUP BY
  player_id,
  joined,
  location,
  age,
  system
```
At this point, we have a table that contains the following information:
- Unique player ID
- Date of last match played
- Date joined 
- Location
- Age
- System

At this point, we need to find out which players were retained and which players were not. As per the instructions, a player's retention is defined by the date of their latest match being at least 30 days after the date they joined. It would have been possible to conduct this calculation using SQL, but I instead chose to bring the entire dataset into sheets so that I could look at unretained players later on as well. Once imported into sheets, we can use a simple formula and IF statement to create a new column and assign values of **yes** and **no** based on whether or not the player's last match took place at least 30 days after their join date. The formula is as follows:

```
=IF((B2-C2)>30, "yes", "no")
```
Once we've created our table, we can begin using pivot tables to conduct our analysis. [The link to the data sheet, pivot tables and charts can be found here](https://docs.google.com/spreadsheets/d/17SnEUZl79uGFn5ROfz6h8IlQySTDKXGg_7ixMdGSarQ/edit?usp=sharing). Our table now has a new column with yes and no labels for each player according to whether or not they would be considered as retained. Creating a pivot table where we count the number of retained players (filtering out ones that were not retained) per each calendar day joined immediately reveals one of the limitations of this defintion, in that we can't determine retention for players who joined in the last 30 days of the year. To plot the rolling 30 day average of retained players, we can count the number of retained players, using filters to remove unretained players, and then using sheets' moving average data trendline, define a 30 day period and add the trendline to the chart. This is depicted in the graph below. 

![image](https://user-images.githubusercontent.com/56803903/140893733-b51435cf-11b0-4a8d-b8c5-a79a525da65e.png)

Notably, the 30 day rolling average begins 30 days into the year, as there wouldn't be enough data previous to that point to correctly calculate the rolling 30 day average.

This retention rolling average got my interested in taking a look at the total number of players who joined on a given day of the year, retained or not, and also taking a look at the 30 day rolling average for that visualization. You may see this in the seet linked above on Pivot tabel 1.2, and charted below. 
![image](https://user-images.githubusercontent.com/56803903/140895558-948772c1-e793-42db-a436-db3ce68b6946.png)

We can see that the rolling average in both cases is relatively flat, and there is no particular month of the year where there is a significant spikes or drop in players joining the game or choosing to continue with the game. 

Finally, I wanted to take a look at whether the players age had any impact on the system they chose to game on. Surprisingly, the split was very even between Linux, Mac and Windows with none of the platforms taking any noteworthy lead in any age groups. This analysis did show a very standard distribution in the age groups that play this game, as depicted below. 

![image](https://user-images.githubusercontent.com/56803903/140888688-642abcce-1471-4370-8ecf-5cfbb95104a3.png)
