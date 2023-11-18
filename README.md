# League of Legends Roles Impact Analysis

by Jeffrey Meredith (jemeredith@ucsd.edu)

---

## Introduction

The dataset I am working with in this project is on professional League of Legends games that happened in 2022. This dataset, before cleaning, has `149_400` rows with 12 rows per game. 2 of those 12 rows are summary stats for each team and the other 10 rows contain data about each player.

The question I am trying to answer in this project is "Which position in League of Legends has the greatest impact on the game's outcome?" This is a question many LoL players might be interested in because choosing the most impactful role allows players to take the course of the game into their own hands.

The relevant columns chosen from the original dataset were `'gameid'`, `'side'`, `'position'`, `'result'`, `'kills'`, `'deaths'`, `'assists'`, `'damagetochampions'`, `'earnedgold'`, and `'visionscore'`. These columns were chosen because they show how well players of various positions did based on their damage, gold income, and K/D/A stats, which are all indicators of a player's impact. The `'gameid'` column was necessary for a merge to be done during data cleaning. Another column `'kdratio'` was engineered by dividing `'kills'` by `'deaths'`. Because some values in the `'deaths'` column are 0, I divided `'kills'` by 1 instead of 0 in those cases to avoid infinite values for `'kdratio'`. Finally, I converted the columns `'kills'`, `'deaths'`, `'assists'`, `'damagetochampions'`, `'earnedgold'`, and `'visionscore'` into differences between the values for red side and blue side per position, resulting in columns `'kd_diff'`, `'kills_diff'`, `'deaths_diff'`, `'assists_diff'`, `'damagetochampions_diff'`, `'gold_diff'`, and `'vision_diff'`. The `'result'` column was converted into a `'winner'` column specifying which side (Red or Blue) won the game.

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

