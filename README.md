# League of Legends Roles Impact Analysis

by Jeffrey Meredith (jemeredith@ucsd.edu)

---

## Introduction

The dataset I am working with in this project is on professional League of Legends games that happened in 2022. This dataset, before cleaning, has `149_400` rows with 12 rows per game. 2 of those 12 rows are summary stats for each team and the other 10 rows contain data about each player.

The question I am trying to answer in this project is "Which position in League of Legends has the greatest impact on the game's outcome?" This is a question many LoL players might be interested in because choosing the most impactful role allows players to take the course of the game into their own hands.

The relevant columns chosen from the original dataset were `'gameid'`, `'side'`, `'position'`, `'result'`, `'kills'`, `'deaths'`, `'assists'`, `'damagetochampions'`, `'earnedgold'`, and `'visionscore'`. These columns were chosen because they show how well players of various positions did based on their damage, gold income, and K/D/A stats, which are all indicators of a player's impact. The `'gameid'` column was necessary for a merge to be done during data cleaning. Another column `'kdratio'` was engineered by dividing `'kills'` by `'deaths'`. Finally, I converted the columns `'kills'`, `'deaths'`, `'assists'`, `'damagetochampions'`, `'earnedgold'`, and `'visionscore'` into differences between the values for red side and blue side per position, resulting in columns `'kd_diff'`, `'kills_diff'`, `'deaths_diff'`, `'assists_diff'`, `'damagetochampions_diff'`, `'gold_diff'`, and `'vision_diff'`. The `'result'` column was converted into a `'winner'` column specifying which side (Red or Blue) won the game.

**Descriptions of Columns**
- `'gameid'` contains a unique ID for each game
- `'position'` specifies the role, whether that be top, jungle, mid, bot, or sup
- `'winner'` specifies whether the winner is `'Red'` or `'Blue'`
- `'kd_diff'` is the difference between Red side's K/D ratio and Blue side's K/D ratio for a given role
- `'kills_diff'` is the difference between Red side's kills and Blue side's kills for a given role
- `'deaths_diff'` is the difference between Red side's deaths and Blue side's deaths for a given role
- `'assists_diff'` is the difference between Red side's assists and Blue side's assists for a given role
- `'damagetochampions_diff'` is the difference between Red side's damage dealt and Blue side's damage dealt for a given role
- `'gold_diff'` is the difference between Red side's gold income and Blue side's gold income for a given role
- `'vision_diff'` is the difference between Red side's vision score and Blue side's vision score for a given role

---

## Data Cleaning and EDA

### Data Cleaning

The first step of data cleaning was to get only the rows with player information instead of having rows with team summary stats. This was done by getting rid of rows where the `'playername'` value was null. Next, I kept only relevant columns that were already mentioned above. Then, I created the `'kdratio'` column by dividing `'kills'` by `'deaths'`, which was also mentioned above as well. Because some values in the `'deaths'` column are 0, I divided `'kills'` by 1 instead of 0 in those cases to avoid infinite values for `'kdratio'`.

Next, the resulting dataframe was split into two dataframes; one contained data about players on Red side and the other contained Blue side data. The two dataframes were then merged into one on the columns `'gameid'` and `'position'` so that each row contains data for each role in the LoL game. Because each role appears twice per game, once for Red and once for Blue, this merged dataframe has almost twice the number of columns. I combined the corresponding columns by taking the Red columns and differencing them with their matching Blue columns. This results in the columns with the suffix `'_diff'` mentioned above: `'kd_diff'`, `'kills_diff'`, `'deaths_diff'`, `'assists_diff'`, `'damagetochampions_diff'`, `'gold_diff'`, and `'vision_diff'`. Since each row was no longer specific to just one side, I changed the `'result'` column with 1s and 0s to a `'winner'` column with values "Red" or "Blue".

**Here is the head of my `cleaned_df`**

| gameid                | league   | teamname                      | position   |   gamelength | winner   |   kd_diff |   kills_diff |   deaths_diff |   assists_diff |   damagetochampions_diff |   gold_diff |   vision_diff |
|:----------------------|:---------|:------------------------------|:-----------|-------------:|:---------|----------:|-------------:|--------------:|---------------:|-------------------------:|------------:|--------------:|
| ESPORTSTMNT01_2690210 | LCKC     | Nongshim RedForce Challengers | top        |         1713 | Red      |  0.333333 |           -1 |            -2 |             10 |                     1687 |        -933 |             4 |
| ESPORTSTMNT01_2690210 | LCKC     | Nongshim RedForce Challengers | jng        |         1713 | Red      |  3.6      |            2 |            -4 |              4 |                    -1201 |        1155 |            -9 |
| ESPORTSTMNT01_2690210 | LCKC     | Nongshim RedForce Challengers | mid        |         1713 | Red      |  1        |            4 |             1 |              9 |                     6432 |        1817 |             0 |
| ESPORTSTMNT01_2690210 | LCKC     | Nongshim RedForce Challengers | bot        |         1713 | Red      |  3.5      |            6 |            -2 |              8 |                    15581 |        3413 |            15 |
| ESPORTSTMNT01_2690210 | LCKC     | Nongshim RedForce Challengers | sup        |         1713 | Red      | -0.2      |           -1 |            -3 |             12 |                      853 |          95 |            -2 |

### Univariate Analysis:

<iframe src="assets/champdmg-hist.html" width=800 height=600 frameBorder=0></iframe>

This is a stacked histogram showing the distribution of Champion Damage differences between Red and Blue side for each position. I notice that the distribution is close to being normal in shape. I also notice that the two positions with the greatest variation in damage difference are bot and mid.

### Bivariate Analysis:

<iframe src="assets/kd-diff-violin.html" width=800 height=600 frameBorder=0></iframe>

This is a multiple violin plot showing the distributions of K/D Ratio differences between Red and Blue for games where Red won and games where Blue won. As expected, when Red wins, the K/D Ratio difference tends to be positive and when Blue wins, it tends to be more negative. The position with the most positive K/D diff when Red wins and the most negative K/D diff when Blue wins, however, is bot lane with a median of 3 when Red wins and a median of -3 when Blue wins.

<iframe src="assets/gold-diff-violin.html" width=800 height=600 frameBorder=0></iframe>

This is a multiple box plot showing the distributions of gold income differences between Red and Blue for games where Red won and games where Blue won. Once again, it can be seen that bot lane has the most positive median gold difference (2579) when Red wins and also has the most negative median gold difference (-2602) when Blue wins.

### Interesting Aggregates:

| winner   |      bot |      jng |      mid |       sup |      top |
|:---------|---------:|---------:|---------:|----------:|---------:|
| Blue     | -3.29655 | -2.08998 | -2.54436 | -0.516813 | -1.94146 |
| Red      |  3.37442 |  1.98933 |  2.55195 |  0.483613 |  1.8846  |

This pivot table shows K/D Ratio difference between Red and Blue for each position for when Red won and when Blue won. It can be seen that the positions with the greatest differences when their side wins are bot lane and mid lane, followed by jungle and top lane, and, lastly, support.

## Assessment of Missingness

### NMAR Analysis

For my NMAR analysis, my goal is to evaluate whether the `'towers'` column is Not Missing At Random. Thinking about the data collection process, I examined the rows of the original dataset where `'towers'` was missing and found that if `'towers'` was missing from one row of a certain game, it was almost always missing from the other rows with that same `'gameid'` as well, except for the rows with team summary stats. This tells me that there were entire games where individual players' towers were not kept track of, which tells me that the missingness doesn't actually depend on the number of towers taken itself, but rather depends on some factors that relate to the match as a whole, whether that be the popularity of the match or which league it took place in. To conclude, because the missingness of `'towers'` does not seem to be dependent on the missing value itself, I can say that `'towers'` is not NMAR.

### Missingness Dependency

The goal of this section was to find what other columns had an effect on the missingness of `'damagetochampions_diff'`. After some permutation tests, it was found that `'damagetochampions_diff'` was **Missing At Random (MAR)** dependent on both `'teamname'` and `'league'`.

First, a permutation test using the Total Variation Distance (TVD) was done to evaluate whether the distribution of `'teamname'` was truly different for when `'damagetochampions_diff'` was missing and when it was not missing. The results as well as the distribution of simulated TVDs from the permutation test and the observed TVD can be seen below.

Observed TVD: **0.499237**
P-Value: **0.0**

<iframe src="assets/team-missing-test.html" width=800 height=600 frameBorder=0></iframe>

Second, a permutation test was done to evaluate whether the distribution of `'league'` was truly different depending on whether `'damagetochampions_diff'` was missing. The results of the permutation test as well as the distribution of simulated TVDs can be seen below.

Observed TVD: **0.462206**
P-Value: **0.026**

<iframe src="assets/league-missing-test.html" width=800 height=600 frameBorder=0></iframe>

**Interpretation:** Since both p-values were less than 5%, we can say that there is evidence that the distributions of both `'teamname'` and `'league'` were different for when `'damagetochampions_diff'` is missing and when it is not missing. This suggests that the missingness of Champion Damage difference is dependent on those other two columns.


## Hypothesis Testing

**Null Hypothesis:** The proportion of games where the bot lane with more damage won is the same as the proportion of games where any other position that had more damage won.

**Alternative Hypothesis:** The proportion of games where the bot lane with more damage won is greater than the proportion of games where any other position that had more damage won.

**Test Statistic:** Difference between proportion of games where bot was leading in damage and won and proportion of games where any other role was leading and won

**Significance Level:** 5%

Below, you can see the results of the permutation test, which was done by shuffling the `'winner'` column to create equal proportions of games won for both bot lane and the other roles when ahead in damage, which is how I simulated the null hypothesis. The simulation was repeated 1,000 times.

Observed Test Statistic: **0.054719**
P-Value: **0.0**

**Conclusion:** We reject the null hypothesis. This means that there is evidence that the proportion of games where the bot lane with more damage won is greater than the proportion of games where any other position that had more damage won. This suggests that bot lane might be the most impactful role on the outcome of a LoL game.

<iframe src="assets/hypothesis-test.html" width=800 height=600 frameBorder=0></iframe>