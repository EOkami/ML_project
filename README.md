#  Song Popularity Prediction (Spotify 2023)

This Machine Learning project analyzes the factors influencing the success of the most-streamed tracks of 2023 on Spotify. The objective is to predict the number of "streams" by combining audio features and reputation data, using a rigorous methodology that ensures the absence of **Data Leakage**.

##  1. Project Objectives
The primary goal is to determine whether a song's success depends on its **intrinsic properties** (rhythm, energy, audio features) or on the **artist's reputation**.

The project demonstrates that an approach based on **Feature Engineering** and an intelligent reformulation of the problem (Target Transformation) yields much more robust and interpretable predictions than using raw initial data.

##  2. Dataset Overview
The dataset includes **953 songs** from 2023 with **24 variables**:
- **Target Variable**: `streams` (total number of plays).
- **Audio Features**: BPM, Danceability, Valence, Energy, Acousticness, Instrumentalness, Liveness, Speechiness.
- **Metadata**: Artists, release date, Key, and Mode.
- **Visibility**: Number of playlists on Spotify, Apple Music, Deezer, and Shazam charts.

###  Data Cleaning & Preprocessing
- **Cleaning**: Data type conversion (converting `streams` from "object" to "numeric").
- **Log Transformation**: Applied $log(1+y)$ to the target variable to reduce the extreme skewness of the stream counts, shifting from a scale of billions to a scale of 0-25 to stabilize variance.

##  3. Exploratory Data Analysis (EDA)
The exploration phase was crucial to validate our modeling assumptions:
- **Outlier Analysis**: Detection of "mega-hit" tracks that skew global averages.
- **Correlation Matrix**: Highlighted a very weak linear correlation between music features alone and success, contrasting with a strong correlation linked to playlist presence.

- **Feature Selection**: Strategic removal of playlist variables for the final model to focus on predictive power available "pre-release."

##  4. Methodology & Engineering (The "Without Leak" Approach)
Managing artist reputation is the technical core of this project to prevent **Data Leakage**.

- **The "Without Leak" Concept**: Using the artist's name can create bias if the calculated popularity includes the performance of the very song we are trying to predict.
- **Technical Solution**: Implementation of the `LeaveOneOutEncoder`. For each song, the model calculates the artist's reputation by averaging the streams of *their other tracks* present in the dataset only.

- **Result**: We obtain a stable measure of notoriety without the model "cheating" by knowing the answer in advance.

##  5. Models Used
We implemented and compared three architectures to address the complexity of the problem:

1. **Linear Regression (Baseline)**: Served as a point of comparison to test for linearity.
2. **Random Forest Regressor**: An ensemble model based on decision trees, capturing non-linear interactions (e.g., the link between tempo and energy).
3. **XGBoost Regressor (Champion)**: An optimized Gradient Boosting algorithm. This is the top-performing model, sequentially correcting prediction errors.


##  6. Performance Comparison
Evaluation was performed using **5-fold cross-validation**:

### Regression Results (RMSLE)
- **Random Forest**: Score of **0.5039**.
- **XGBoost**: **0.4885** (Best performance).

### Classification Results (F1-Score)
*Predicting if a song belongs to a popularity tier (Low, Medium, High).*
- **Random Forest**: **0.8315**.
- **XGBoost**: **0.8441** (Best performance).

**Feature Importance**: Artist Reputation (Without Leak) and Song Age are the dominant factors for success.

##  7. Repository Structure
```text
├── Projet_ML_N.ipynb   # Main notebook (Cleaning, EDA, Models)
├── spotify-2023.csv         # Source dataset
└── README.md                # Project documentation
