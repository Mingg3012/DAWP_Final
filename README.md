# Sport Match Outcome Prediction

## 1. Project Overview

This project applies a complete data analysis and machine learning workflow to predict European football match outcomes using the European Soccer Database. The prediction task is formulated as a three-class classification problem:

- `H`: Home Win
- `D`: Draw
- `A`: Away Win

The project focuses on data cleaning, exploratory data analysis, feature engineering, model training, and model evaluation. The final goal is to build a predictive model that uses only pre-match information to forecast football match results.

## 2. Dataset

The dataset used in this project is the European Soccer Database from Kaggle:

**Dataset:** [European Soccer Database](https://www.kaggle.com/datasets/hugomathien/soccer)

The original dataset is stored in SQLite format and contains information about:

- Matches
- Teams
- Players
- Player attributes
- Team attributes
- Leagues and countries
- Betting odds
- Match events

## 3. Project Workflow

The project follows the main stages below:

1. Data loading from SQLite database
2. Data cleaning
3. Exploratory Data Analysis
4. Feature engineering and preprocessing
5. Model training and hyperparameter tuning
6. Model evaluation and comparison
7. Final model selection

## 4. Data Cleaning

The cleaning process was performed to ensure that the dataset was reliable and suitable for EDA and modeling.

Main cleaning steps include:

- Handling missing values based on column type and meaning
- Selecting Bet365 as the main bookmaker odds source
- Removing rows with missing essential match information
- Applying KNN imputation for numerical player and team attributes
- Applying simple imputation for categorical attributes
- Removing event-related columns to prevent data leakage
- Checking invalid values such as negative goals or invalid team IDs
- Treating numerical outliers using the IQR rule and winsorization

Post-match variables such as final goals and in-match events were not used as model inputs because they would not be available before kickoff.

## 5. Exploratory Data Analysis

EDA was conducted to understand the structure of the data and guide feature engineering decisions.

Main findings:

- Home wins are the most frequent outcome, showing a clear home advantage.
- Draw is the most difficult class to classify.
- Outcome distributions vary across seasons and leagues.
- Bookmaker odds are strong pre-match indicators.
- Player and team attribute differences provide useful relative-strength signals.
- Some engineered features are highly correlated, especially player and team attribute variables.

These findings influenced the creation of features such as odds probabilities, Elo ratings, recent form, draw tendency, player deltas, team deltas, and league/time context features.

## 6. Feature Engineering

The final feature matrix combines cleaned raw pre-match features and selected engineered features.

Main feature groups:

- Bet365 odds features
- Normalized implied probabilities
- Odds uncertainty features
- Elo rating features
- Recent form features
- Draw tendency features
- Rest and schedule congestion features
- Player attribute delta features
- Team attribute delta features
- League and season context features

The final dataset contains:

| Metric | Value |
|---|---:|
| Rows after filtering | 19,691 |
| Final input features | 260 |
| Missing-indicator columns | 96 |
| Target classes | A, D, H |

## 7. Modeling

Two tree-based models were implemented:

### Decision Tree

Decision Tree was used as an interpretable baseline model. It can capture non-linear decision rules but is more sensitive to overfitting.

### Random Forest

Random Forest was used as the main ensemble model. It reduces the variance of a single decision tree by combining many trees and generally provides more stable predictions.

Hyperparameter tuning was performed using `TimeSeriesSplit` with 3 folds on the training set. The main selection metric was Macro-F1 because the target classes are imbalanced and the Draw class is difficult to predict.

## 8. Evaluation Results

The models were evaluated on a chronological hold-out test set.

| Model | Macro-F1 | Balanced Accuracy | Accuracy |
|---|---:|---:|---:|
| Random Forest tuned | 0.4744 | 0.4778 | 0.4939 |
| Decision Tree tuned | 0.4592 | 0.4623 | 0.4697 |
| Bookmaker B365 baseline | 0.3858 | 0.4462 | 0.5278 |
| Random baseline | 0.3245 | 0.3297 | 0.3294 |
| Majority baseline | 0.2064 | 0.3333 | 0.4485 |

The tuned Random Forest achieved the best Macro-F1 and balanced accuracy, so it was selected as the final model.

Although the Bookmaker B365 baseline achieved higher raw accuracy, its Macro-F1 was lower because it performed poorly on the Draw class. This confirms that accuracy alone is not sufficient for evaluating this task.

## 9. Key Insights

The most important predictive signals came from:

- Bet365 implied probabilities
- Odds uncertainty features
- Player quality differences
- Team strength differences
- Elo-based features
- Match balance indicators

The Draw class remained the most difficult outcome because draws often occur when two teams are close in strength and the statistical boundary between a draw and a narrow win is very small.

## 10. Technologies Used

- Python
- SQLite
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- Jupyter Notebook

## 11. How to Run

1. Clone this repository:

```bash
git clone https://github.com/your-username/your-repository.git
cd your-repository
