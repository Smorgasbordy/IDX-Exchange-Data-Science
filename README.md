# IDX Exchange - Data Science Internship: Property Price Prediction
This repository contains the code that I wrote during my 12-week data science internship with IDX Exchange.
During this internship, I cleaned, preprocessed, and feature engineered real-world property data, and then used
linear regression, decision tree, random forest, and gradient boosted models to predict the sale price of single-family residential properties in California. For a full breakdown of what actions I took every week, refer to week_by_week_summary.md.

## Dataset
The data I used for this project was pulled from IDX Exchange's California Regional Multiple Listing Service (CRMLS) database. This database contains monthly information on California properties which have been sold, with features relating to property qualities (beds, baths, lot size, etc.), location (county, city, postal code), and listing/sold information (listing price, close price, days on market, etc.). While the CRMLS contains information on many different property types and subtypes (commercial, farm, etc.), per the instructions of the internship, only listings with `PropertyType = "Residential"` and `PropertySubType = "SingleFamilyResidence"` were used.
* **Dataset Size**: (309735, 83)
* **Scope**: January 2024 - April 2026, 28 months total
* **Target Variable**: `ClosePrice`

## Data Preparation
### Train/Validation/Test Split
Unlike many data science projects, the training, validation, and test sets were not chosen on a random split of the data. Instead they were divided as so:
* **Train Set**: January 2024 - February 2026, 26 months
* **Validation Set**: March 2026, 1 month
* **Test Set**: April 2026, 1 month

Additionally, while the validation and test sets always stayed the same, sometimes the train set was truncated to test model efficacy on different training windows. That being said, all reported metrics in this file use the full training window above.

### Preprocessing
* **Removing Data Leakage**: The original dataset included information that would only be known after the property was listed or sold, such as `ListPrice`, `DaysOnMarket` or `ListAgentName`. Since we want the model to work on any property, regardless of listing status, any information that would come from a listing, and wasn't an inherent feature of the property, needed to be deleted.
* **Dropping Unnecessary Data**: Some columns were dropped for redundancy, irrelevance, or high missingness. For example, several redundant columns encoded lot size, but used different units. `LotSizeSquareFeet` was kept, while `LotSizeDimensions`, `LotSizeArea`, and 'LotSizeAcres` were dropped. Metadata fields like `OriginatingSystemName` were also removed. Additionally, columns with >50% missing rows were dropped (after being checked for relevancy).
* **Handling Outliers**: Numeric features were winsorized (most using the 0.95 percentile as the fill value), except for `ClosePrice` which, following the internship directions, had its <0.5 percentile and >0.95 percentile values dropped entirely. To avoid data leakage, outlier cutoffs were learned only on the training data and then applied to the test set.
* **Filling Missing Data**: Missing boolean and categorical values were filled with "Unknown". Missing numerical features were imputed through various methods (check `02_preprocessing.ipynb` for more detail). Missing `Latitude`, `Longitude` and 'City` values were filled by matching their `PostalCode` values to the uszipcode database (https://pypi.org/project/uszipcode/).
* **Encoding Categorical Values**: Low cardinality features like `Flooring` and boolean columns were one hot encoded. High cardinality features like `PostalCode` and `HighSchoolDistrict` were target encoded, making sure to avoid data leakage by using only the encodings learned from the training data and applying those to the test set.

After this process, the three cleaned and workable datasets were:
* **Train Set**: (255033, 47)
* **Validation Set**: (10293, 47)
* **Test Set**: (11327, 47)

### Feature Engineering
Some custom features were created to help improve model performance:
* `BedBathRatio`: computed from `BedroomsTotal / BathroomsTotalInteger`
* `PropertyAge`: computed from the date the property was entered into the MLS system  - `YearBuilt`
* School District Information: the original dataset had a lot of missing `HighSchoolDistrict` values. To remedy this, the dataset was joined (using longitude + latitude) with the California School District Areas 2024-25 dataset (https://data.ca.gov/@california-department-of-education/california-school-district-areas-2024-25). This provided far more accurate school district information, as well as the type of district (high, elementary, unified), public/private enrollment counts, and demographic data.

## Modeling
Once that dataset was cleaned and engineered, it could be tested on various machine learning models. Many variations of each model were tested, and their hyperparamters likewise tuned. The best performing model of each type is displayed below. Furthermore, all reported metrics represent performance on the test set:

| Model | R² score | MAPE | MdAPE | MAE |
| ---- | -------- | ----- | ------ | ---- |
| Linear Regression | 0.855653 | 12.550245 | 9.552526 | $148,375 |
| Decision Tree | 0.867673 | 11.783563 | 8.438892 | $140,170 |
| Random Forest | 0.905958 | 9.683137 | 6.796715 | $116,191 |
| XGBoost | 0.919499 | **8.893307** | 6.240518 | $106,460 |
| **XGBoost (no log transform)^** | **0.921226**| 9.076099 | **6.285586** | **$106,422** |

^All models were designed to predict `ClosePriceLog`, the log transform of `ClosePrice`. The predicted values were then transformed back to their normal state for evaluation. For Linear Regression, Decision Trees, and Random Forests, this was found to improve performance. However, for the XGBoost model, predicting `ClosePrice` rather than `ClosePriceLog` resulted in better performance for all metrics besides MAPE.

## Evaluation
The metrics listed above were used throughout the project to evaluate each model. However, to get a better understanding of the model, more in-depth evaluation methods were tested. 
* Across all models, `PostalCode` was shown to be the most important feature, reinforcing the famous notion of "Location, Location, Location" in real estate.
* By plotting predicted vs. true values, it was discovered that most model error occurs when models under-predict very expensive `ClosePrice`.
* Using price band analysis, it was found that the models perform the best for cheaper homes, with model performance decreasing as `ClosePrice` increases.
* Likewise, models tend to perform worse for counties with high median `ClosePrice`, such as Santa Cruz and San Fransisco.

#### Price Band Analysis of XGBoost Model
Price bands were chosen based on data percentiles: [0, 0.25), [0.25, 0.5). [0.5, 0.75), [0.75, 1]
| Price Band | Test MAPE | Test MdAPE |
| ---------- | ----- | ----- |
| <$650K | 7.992131 | 4.695446 |
| $650K-$890K | 7.514709 | 5.011109 |
| $890K-$1.35M | 9.372293 | 6.945837 |
| $1.35M+ | 10.354906 | 8.306254 |

## Streamlit App
A simplified version of the final XGBoost model was used to create an interactive app in streamlit. Users input a living area, lotsize, bed/bath count, and city, and receive a predicted close price. The model behind the app is only trained on `LivingArea`, `LotSizeSquareFeet`, `BedroomsTotal`, `BathroomsTotalInteger`, and `City` and performs with an  R² score of 0.82. However, this simplification is necessary for ease of use. Each of the chosen features are readily available for any online listing, while other features of the finalized model (like `YearBuilt` or `AssociationFee`) can be harder to find.

To run the app yourself, use `app.py` in the streamlit folder. Make sure to download `model.joblib` and `city_encoder.joblib` so that your device has access to the trained model.

## Repository Guide
```
01_exploration.ipynb              Week 2 - EDA
02_preprocessing.ipynb            Week 3 - Cleaning, Encoding
03_baseline_model.ipynb           Week 4 - Linear Regression
04_model_comparison.ipynb         Week 5 - Decision Tree, Random Forest
Feature_Engineering.ipynb         Week 6 - Added Features
05_advanced_models.ipynb          Week 7 - XGBoost
06_evaluation.ipynb               Week 8 - Feature Importance, Pred/Truth Plot, Price Bands, Location
metrics_summary                          - Performance Summary of Each Model

Streamlit Folder                  Week 9 - Streamlit Prediction App
- app.py                                 - Streamlit Script
- create_streamlit_model.ipynb           - Simplified Model Training & Evaluation
- model.joblib                           - Model Download
- city_encoder.joblib                    - Encoder Download

Text Files                        Week 10 - Documentation
- README.md                               - Key Findings
- week_by_week_summary.md                 - Weekly Summary of Findings
```
