# UCB ML/AI Certification: Practical Assignment 11.1
## Problem Statement

The problem is to find features in a dataset that determines the price and sellability of used cars.  
The goal of the assignment is to find the factors that determine car price and to provide clear recommendations to the client -- a used car dealership -- as to what consumers value in a used car.
  
## Methodology  

The CRISP-DM Methodology was used for this exercise. In the Data Understanding and Data Preparation steps, several insights were gained and we were able to drop columns and outlier rows. The data was also cleaned up to remove outliers, and some columns with high cardinality were reduced to a lower cardinality. In the modeling step, grid searches were perfomed using various hyperparameters and estimators.

## Data Understanding

The data set provided has 426880 entries in 18 columns. The data was explored by reviewing key stats about the data - NaN entries by feature and unique entries in each feature. I plotted historgrams of key features.  
This exercise provided a wealth of information for the next step, data preparation.  

## Data Preparation

The data has 18 features and 426880 entries. Examination and plotting of the data indicate several opportunities for improving data quality and removing outlier data points. 

##### Column Removal  
* some columns either have unique values in the data set or excessively high cardinality.  
* 'id' and 'VIN' are unique and will not contribute to the saleability or price. These can be dropped.  
* 'region' has high cardinality and can be dropped. The 'state' and 'census_region' will cover for this feature.  
* The charts indicate that 'title_status' is mostly "clean". This feature can be dropped.  
* The charts indicate that 'fuel' is mostly "gas". The rows with other values should dropped, and then this feature can be dropped.  
* 'paint_color' was insignificant in the analysis and has many NaN values. Drop the feature.
    
##### Row removal  
* Certain features are critical: price, year, manufacturer, model, odometer. Drop the rows where these features have NaN 
* Drop rows that have outlier 'price.' (arbitrarily defined as <3000$ and >100000$)  
  
##### NaN  
* There are a significant number of NaN entries, especially in the following features: 'condition', 'cylinders', 'drive', 'size', 'type', and 'paint_color'. It seems that dropping all the NaN data rows will affect the data size, but for simplification, I have chosen to drop rows having >3 NaNs (which has no effect on this data). 
* Columns that have more than a third of the data as NaN can also be removed for simplification
* Further actions of NaN rows can be used to optimize compute time.
  
##### Improving focus by removing some categories and reducing cardinality   
* The data for 'year' is too widespread. Obviously, 1950 automobiles will not impact the price or sellability of more recent cars. A fair approximation is removing all data with 'year' < 2001.
* 'model' has 29000+ unique values. The data is really complex and unclean. The one hot encoder and dataframe size exploded. I decided to keep the top 25 models as-is and change the rest to "other"
* Similarly, 'manufacturer' with less than 5000 data rows can be dropped to avoid noise in the regression.  
* Map states to geographic regions based on us census data

##### Column Data Observation    
* 'year' and 'odometer' have numerical data. The rest of the columns have categorical data and will need One Hot Encoding (OHE).    
* 'model' and 'odometer' have high unique data. These may need clustering before OHE.  
* The 'cylinders' column may be converted to a numerical category by mapping values.  

##### More Data Preparation

1. Impute for missing values, both numerical and categorical features
2. One Hot Encoding for categorical features
3. Scale the data to normalize

These can be done in the data preparation step here or as part of the pipeline during regressions. 
Since the regressions will be run multiple times, it is better to do it before to save run time.

After this, the dataframe will be ready to be split into train and test data sets
  
## Modeling

## Evaluation

## Deployment
The Jupyter Notebook for this assignment is checked into the Git Hub repository at https://github.com/rvraj26/Assignmen11.1_RajVarada
  
## Results

## Future Work
1. Enhance the code to identify the features are a good predictor of a used car sale, given the attributes of a customer and an automobile
2. Enhance the NaN handling. Have to add a method to add default values for missing data. Add more methods to reduce the amount of data removed due to NaN 
3. Address and remove the simplifying assumptions during data preparation  
