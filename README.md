# Model Card: Soccer Player Injury Prediction Model

## Model Details

- **Model name:** Soccer Player Injury Prediction Model
- **Model author:** Yasir Mohammad, Data Scientist, [GitHub Profile](https://github.com/yasirbm)
- **Model version:** v1.0
- **Model type:** Binary Classification
- **Model implementation:** XGBoost
- **Model code:** [Python Model](https://github.com/yasirbm/soccer_player_injury_model/blob/main/soccer_injury_model.ipynb)


## Intended Use

This model is intended to predict the likelihood of a soccer player getting injured in the next game based on various features related to the player, including playtime, injury history and physical attributes. The model can be used by team managers and coaches to help inform them on the risks of selecting a given player for an upcoming game.

## Data Source(s)
The following data sources were used for this model:
- Player attributes - [FIFA 16-21 data] (https://www.kaggle.com/datasets/stefanoleone992/fifa-21-complete-player-dataset?select=players_20.csv)
- Injury history - Transfermarkt injury history data. Pulled and scraped from there using [worldfootballR R package] (https://rdrr.io/cran/worldfootballR/)
- Player game time data - [Barclays Premier League Soccer Dataset](https://www.kaggle.com/datasets/narekzamanyan/barclays-premier-league?select=player_stats.csv)

## Players/seasons in scope:

- Original scope was all players who have played in the British Premier League at any point between 2016/17 season and 2020/21 season
- Due to complications and difficulties in joining 3 datasets from entirely different sources, this came out to a total of 685 rows of data, consisting of 317 players

## Training Data

- 3 separate data sources were combined to create a datset which included player attributes (i.e. - pace, height, weight), player injury history and player game time
- Data was grouped on a player-year level
- Data was split 70:30 train:test. Training data consisted of 479 rows with 12 features
- Since a positive value in the target variable only accounted for ~25% of the data, the data was oversampled the training data using SMOTE (Synthetic Minority Oversampling Technique)
- The final training dataset (after SMOTE) consisted of 734 rows
- Target variable: 'major_injury' - binary variable based on whether player was injured for more than 120 calendar days in a given season
- Data Dictionary:

| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
|**p_id2**| ID | string | unique player indentifier |
| **start_year** | input | int | start year of current season |
| **height_cm** | input | float | player's height in centimetres averaged across FIFA 16-21 |
| **weight_kg** | input | float | player's weight in kilograms averaged across FIFA 16-21 |
| **work_rate_numeric** | input | ordinal | player's work-rate mode across FIFA 16-21 ranging from 2-4 in 0.5 intervals |
| **pace** | input | int | player's 'pace' rating averaged across FIFA 16-21 |
| **physic** | input | int | player's 'physical' rating averaged across FIFA 16-21 |
| **position_numeric** | input | nominal | 'Goalkeeper': 0, 'Defender': 1, 'Forward': 2, 'Midfielder': 3 |
| **age** | input | int | player age in relevant season |
| **cumulative_minutes_played* | input | int | cumulative minutes played by player in all previous seasons |
| **minutes_per_game_prev_seasons**| input | int | average minutes played per game in all previous seasons |
| **avg_days_injured_prev_seasons**| input | int | average calendar days injured in all previous seasons |
| **significant_injury_prev_season**| input | int | whether player had significant injury in previous season; 1 = yes; 0 = no |
| **cumulative_days_injured**| input | int | cumulative calendar days player was injured prior to current season |
| **target_major_injury**| target | bool | whether player had major injury in current season; True = yes; False = no |

## Test Data

- 70/30 train:test data split on original data set of 685 samples
- Hence, test data consisted of 206 samples

## Parameters

After a grid search, the model arrived at the following optimal hyperparameters:

- Number of trees: 200
- Maximum depth per tree: 4
- Learning rate: 0.1
- Minimum child weight: 1
- Subsample (columns): 0.8
- Subsample (rows): 0.9

## Feature Importance
![image](https://user-images.githubusercontent.com/89418186/231194894-6de126fe-e712-483d-a7b3-352474f1256c.png)

## Example of one tree
Below is an example of one of the 200 decision trees involved in the XGBoost model
![image](https://user-images.githubusercontent.com/89418186/231194934-4cca25e0-ecdb-4540-bf13-63c92d00e314.png)

## Metrics

- Accuracy: 73.30%
- Precision: 26.83%
- Recall: 30.56%
- F1 score: 28.57%
- AUC-ROC score: 66.65%

![image](https://user-images.githubusercontent.com/89418186/231194983-63ba7c19-de52-4d40-aa24-619395e598de.png)

## Ethical Considerations

- The use of this model should be limited to help inform coaches on potential risks of playing certain players
- The model does not consider how certain features (i.e. Physical attributes) may be correlated with demographics such as race or nationality 
- The model should not be used to discriminate against players based on any personal characteristics

## Limitations and Bias

- Relatively small dataset due to scarcity in available data for the features I was looking at. A model like XGBoost generally performs better with a larger sample size 
- This focuses only on the premier league, which is just one of hundreds of leagues around the world 
- The model focuses on Men’s football. The features that contribute to Women’s injuries may be entirely different

## Licensing

This model is licensed under the Apache License.
