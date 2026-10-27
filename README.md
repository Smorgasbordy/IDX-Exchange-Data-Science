# IDX Exchange - Data Science Internship
This repository contains the code that I wrote during my Fall 2026 IDX Exchange Data Science Internship. 
This internship focuses on cleaning, analyzing, and feature engineering MLS real estate data, and then using this data
to create linear regression, decision tree, and random forest models to predict California housing prices.

## Week One: Orientation & Setup
Week One simply required downloading the datasets and understanding the type of data being used.

### Tasks:
* Confirm understanding of key columns. Notes found here: https://docs.google.com/document/d/1cgUxa82GOcS5UUH0uHAOx46Gpzj5-TucTl0R3u1x16Q/edit?usp=sharing

## Week Two: Data Exploration
Week Two was about performing basic EDA on the sold dataset by visualizing key features against ClosePrice (the target variable).

### Tasks:
* Load and aggregate sold datasets.
* Filter dataset to PropertyType = "Residential" and PropertySubType = "SingleFamilyResidence."
* Visualize the distributions of ClosePrice (target variable) against LivingArea, BedroomsTotal, BathroomsTotalInteger,
  and LotSizeSquareFeet.
* Remove outliers for better visualization and mark for proper handling later.
* Note visible correlations between features and target variable.
* Deliverable: 01_exploration.ipynb

## Week Three: Data Preprocessing
Week Three focuses on preparing the data for a linear regression model. This involves handling missing values
and outliers, as well as converting categorical data to numerical.

### Tasks:
* Remove impossible values (ex: ClosePrice <= 0 or CloseDate <= ListDate).
* Drop columns that have a majority of their values missing.
* Prevent leakage by removing columns that are only known after a property has been sold.
* Handle remaining missing values via deletion, imputation, or flagging as "unknown."
* Process numerical data by removing outliers and normalizing.
* Convert categorical features to numerical.
* Create a train/test split, reserving the testing set as the most recent month in the data.
* Deliverable: 02_preprocessing.ipynb

## Week Four: Baseline Model
In week four I created a baseline Linear Regression model to compare future improved models against. This baseline model achieved an
r² score of 0.845539.

### Tasks
* Test the cleaned data set by running it through basic Linear Regression.
* Evaluate this model using r² scores.
* Make tweaks to the regression input in order to improve performance. Applied improved target encoding and outlier winsorization.
* Deliverable: 03_baseline_model.ipynb

## Week Five:  Additional Models
In week five I tested my data on Decision Trees and Random Forests, once again evaluated using r² scores. The best performing
Decision Tree got an r² score of 0.863098, which is an improvement on the Linear Regression model. The best performing Random Forest
had an r² score of 0.902822, a definite increase compared to the solo Decision Trees.

### Tasks:
* Test the baseline dataset on Decision Trees and Random Forests.
* Evaluate the models using r² scores.
* Make tweaks to the dataset to better suit these non-linear models.
* Tune hyperparameters such as max_depth, min_samples_leaf, and n_estimators.
* Deliverable: 04_model_comparison.ipynb

## Week Six: Feature Engineering
Week six was about adding engineered features to the dataset and then rerunning the linear regression and decision tree / random forest models using these new features. Two new features were created, BedBathRatio and PropertyAge. Additionally, the MLS dataset was joined with the California School District Areas 2024-25 dataset (https://data.ca.gov/@california-department-of-education/california-school-district-areas-2024-25) to improve the accuracy of the school district information. With the addition of these new features, the r² scores of the linear regression, decision tree, and random forest models were increased to 0.855653, 0.867673, and 0.905958 respectively.

### Tasks:
* Create the new features BedBathRatio and PropertyAge.
* Join the MLS dataset with the California School District Areas 2024-25.
* Make sure the newly joined dataset is properly formatted, fill missing values, and encode categorical features.
* Rerun the linear regression and tree models on the new dataset and record the metrics.
* Deliverable: Feature_Engineering.ipynb for new dataset, updated 03_baseline_model.ipynb and 04_model_comparison.ipynb for new models
