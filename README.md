# IDX Exchange - Data Science Internship
This repository contains the code that I wrote during my Fall 2026 IDX Exchange Data Science Internship. 
This internship focuses on cleaning, analyzing, and feature engineering MLS real estate data, and then using this data
to create linear regression, decision tree, and random forest models to predict California housing prices.

## Week One:
Week One simply required downloading the datasets and understanding the type of data being used.

### Tasks:
* Confirm understanding of key columns. Notes found here: https://docs.google.com/document/d/1cgUxa82GOcS5UUH0uHAOx46Gpzj5-TucTl0R3u1x16Q/edit?usp=sharing

## Week Two:
Week Two was about performing basic EDA on the sold dataset by visualizing key features against ClosePrice (the target variable).

### Tasks:
* Load and aggregate sold datasets.
* Filter dataset to PropertyType = "Residential" and PropertySubType = "SingleFamilyResidence."
* Visualize the distributions of ClosePrice (target variable) against LivingArea, BedroomsTotal, BathroomsTotalInteger,
  and LotSizeSquareFeet.
* Remove outliers for better visualization and mark for proper handling later.
* Note visible correlations between features and target variable.
* Deliverable: 01_exploration.ipynb

## Week Three:
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

## Week Four:
In week four I created a baseline Linear Regression model to compare future improved models against. This baseline model achieved an
r² score of 0.888.

### Tasks
* Test the cleaned data set by running it through basic Linear Regression.
* Evaluate this model using r² scores.
* Make tweaks to the regression input in order to improve performance. Applied improved target encoding and outlier winsorization.
* Deliverable: 03_baseline_model.ipynb

## Week Five:
In week five I tested my data on Decision Trees and Random Forests, once again evaluated using r² scores. The best performing
Decision Tree got an r² scsore of 0.9, which is comparable to the Linear Regression model. The best performing Random Forest
had an r² score of 0.923, an improvement upon the solo Decision Trees.

### Tasks:
* Test the baseline dataset on Decision Trees and Random Forests.
* Evaluate the models using r² scores.
* Make tweaks to the dataset to better suit these non-linear models.
* Tune hyperparameters such as max_depth, min_samples_leaf, and n_estimators.
* Deliverable: 04_model_comparison.ipynb
