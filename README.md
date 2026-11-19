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
Additionally, while the validation and test sets always stayed the same, sometimes the train set was truncated to test model efficacy on different training windows.

### Preprocessing
### Feature Engineering

## Modeling

## Evaluation

## Streamlit App
