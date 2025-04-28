# 4IZ451

Project to classify NBA shot data - whether it was a made shot or missed shot. We used unofficial NBA API (https://github.com/swar/nba_api) to gather data. This repository contains code from initial data collection stage as well as final classification iplementation.

Repository structure:
 - data_prep folder contains jupyter notebook data_prep.ipynb which was used to create dataset, as well as elementary datasets which were combined into final
 - report.ipynb is the notebook containing data exploration and modeling
 - final_data_spurs.csv is final dataset

## Data preparation

The initial idea was to use league-wide play-by-play data, so that we would have accessible every shot taken during the 2024-25 season. This turned out to be quite difficult to achieve through the unofficial API. The final dataset contains shots taken in all games of a single team during the 2024-25 season - the San Antonio Spurs. The dataset was prepared in three steps:

1. Play-by-play data collection was achieved using playbyplayv3 endpoint (https://github.com/swar/nba_api/blob/master/docs/nba_api/stats/endpoints/playbyplayv3.md).
2. League-wide shot chart data in order to fetch shooting percentages for all players from specific areas of the court. Using endpoints shotchartdetail (https://github.com/swar/nba_api/blob/master/docs/nba_api/stats/endpoints/shotchartdetail.md) and shotchartleaguewide (https://github.com/swar/nba_api/blob/master/docs/nba_api/stats/endpoints/shotchartleaguewide.md).
3. Player shooting percentage was extracted from endpoint playerdashptshots (https://github.com/swar/nba_api/blob/master/docs/nba_api/stats/endpoints/playerdashptshots.md). This way we could access detailed player shooting stats.

## Dataset
- SHOT_VALUE - whether shot was a 2-point or 3-point attempt, free throws are excluded
- 'ACTION_ID'
- SCORE_DIFF - difference in score from shooting player team perspective before shot is taken, e.g. -2 = shooting player team was losing by two points at time of shot being taken
- CLUTCH_FLAG - flag whether shot was taken in "clutch time", defined by NBA as less then 5 minutes remaining in fourth period or OT and score difference within 5 points
- GAME_ID
- PLAYER_ID
- PLAYER_NAME
- TEAM_ID
- ACTION_TYPE - description of shot type
- SHOT_ZONE_BASIC, SHOT_ZONE_AREA, SHOT_ZONE_RANGE - these variables together define specific areas of the court, used to calculate ZONE_FG_PCT
- ZONE_FG_PCT - league-wide shooting percentage from area of court where shot was taken (based on SHOT_ZONE_BASIC, SHOT_ZONE_AREA, SHOT_ZONE_RANGE)
- SHOT_DISTANCE - distance from to shooting player to basket, in feet
- LOC_X - x axis location where shot was taken
- LOC_Y - y axis location where shot was taken
- SHOT_MADE_FLAG - target variable
- FGA_FREQUENCY
- FGM - count of Field Goals Made
- FGA - count of Field Goals Attempted
- FG_PCT - Field Goal Percentage
- EFG_PCT - Expected Field Goal Percentage
- FG2A_FREQUENCY
- FG2M - count of 2-point Field Goals Made
- FG2A - count of 2-point Field Goals Attempted
- FG3A_FREQUENCY
- FG3M - count of 3-point Field Goals Made
- FG3A - count of 3-point Field Goals Attempted
- PLAYER_SHOT_PCT - value of player shooting percentage in season (FG2_PCT if SHOT_VALUE is 2 and FG3_PCT if SHOT_VALUE is 3)


## Model training
- Features:
    - Numercial: 'SHOT_VALUE', 'SCORE_DIFF', 'CLUTCH_FLAG', 'SHOT_DISTANCE', 'LOC_X', 'LOC_Y', 'ZONE_FG_PCT', 'FG_PCT', 'EFG_PCT', 'PLAYER_SHOT_PCT'
    - Categorical: 'ACTION_TYPE'
- Target variable: 'SHOT_MADE_FLAG'