# CricketProject
## Sabermetric analysis of IPL data

I wanted to analyse cricket data in a similar way to how baseball data is analysed. There are a few challenges with this, mainly that there has not been much work done in this field before, and that cricket has a lot of hidden variables that affect the game, such as the quality of the pitch or the size of the ground. While there have been no groundbreaking conclusions so far, I have tried to tell a story about the game through an analytical lense. The data I used was from Kaggle, though I built a scraper to get match statistics from Cricbuzz. 


### In the IPL-Agg file:

The first thing I did was aggregate the number of runs scored by each team in each innings, and add it to my dataframe of aggregates (match_df). I then filtered out all the matches that were affected by rain, had no result, or were tied. Since the chasing team has to stop batting when they get more runs than the team batting first, I gave them bonus runs for finishing with extra balls left. I then looked at season by season data, and looked at avg score batting 1st, average adjust score batting 2nd, win rate of team batting 1st, and the ratio at which the team batting 1st scored to team batting 2nd. 

![Season Table](https://github.com/molron94/CricketProject/blob/master/Season%20Aggregate%20Table.png)

Above you can see that the team that batted first scored faster and won less, which means that speed of run scoring is not a sensible way to compare batting 1st and 2nd. You can also see that average scores increased significantly 2014 and after, which likely had to do with more innovative batting techniques within the game. Therefore, I chose to focus my analysis on the team batting first 2014 and after (222 data points). 


I examined the relationship between win rate of team batting first, and scoring rate, i.e. how much faster the team batting first scored runs. These variables were highly correlated (.82), so I ran a linear regression on them, and plotted the result on a scatter plot. I also plotted a bar graph with the estimated win rate, and real win rate. Finally, I bucketed different scores batting first 2014 and after, and plotted the average score and win probability of each bucket on a scatter plot. 


### In the IPL-Over Stats
##### Target Variable: first innings runs

I ran the same filters that I did in the Agg file. I also added over by over data about dots, runs, wickets to match_df. I also added cumulative stats about runs, dots, wickets upto a certain point. I then created 4 quarters of the innings to aggregate. I first ran a regression to find out the cost of a wicket in each quarter of the innings. I found a fairly smooth linear decay, and this decay was to be expected. The cost of wickets in the last quarter was fit to only 3.2. I wondered if this was because teams that were doing well till this point took more risks, and thus lost more wickets, skewing the data. This turned out not to be the case, as I found no relationship between wickets lost in the first 3 quarters and wickets lost in the fourth quarter. 

I then ran a regression with both wickets and dots, and while the relationship between wickets remained linear, the cost of a wicket declined sharply (by 35-50%), suggesting that a big cost of wickets were additional dot balls, likely because of a fear of future wickets. The cost of a dot ball while similar in the first and second quarters, increased sharply in both the third and fourth quarters. In a fourth quarter of the innings, my regression suggested that a wicket falling was less significant, than the fact it likely fell off a dot ball! 

Finally, I ran a regression of wickets in each quarter vs the total number of dot balls, i.e. to see the number of dot balls each wicket creates. I once again found a linear decline, with each ball having a roughly 24% chance (29/120) of being a dot ball, without any wickets in the innings. A wicket in the first quarter added 4.35 dot balls, whereas a wicket in the last quarter added roughly 0.8. The reason it is less than 1, is because we need to add the chances of a dot ball (0.24) to the 0.8 number, giving us that each wicket (assuming it is on a dot ball), creates only 0.04 extra dot balls, in addition to the wicket delivery. This suggests that a new batsman does not need much time to get set at the end of an innings.


## Conclusions
My data seems to support a negative linear relationship between the cost of a wicket and number of balls left in an innings. It also seems to support a positive linear relationship between the number of dots created by a wicket and time left in the innings. Finally, there is also a clear increasing relationship between the cost of a dot ball and time left in the innings, which is also backed by higher scoring rates at these times.

While these results are intriguing, it must be remembered that my sample size is small, so it is likely more data needs to be gathered to explore conclusions further.

### Next Steps
1) Gather more data
2) Develop some analysis of the 2nd innings
3) Player specific analysis, e.g. form/ match ups
4) Understand, and try to isolate the effects of pitch condition/ground size

