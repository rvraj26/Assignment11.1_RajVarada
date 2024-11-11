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

The data has 18 features and 426880 entries. The data examination and plotting indicate several opportunites for improving data quality and removing outlier data points. 
  
#### Column Removal  
* There are some columns which either have unique values in the data set or have excessively high cardinality.  
* 'id' and 'VIN' are unique and will not contribute to the saleability or price. These can be dropped.  
* 'region' has high cardinality and can be dropped. The 'state' and 'census_region' will cover for this feature.  
* The charts indicate that 'title_status' is mostly "clean". This feature can be dropped.  
* The charts indicate that 'fuel' is mostly "gas". The rows with other values should dropped and then this feature can be dropped.  
  
#### Row removal  
* Drop rows that have zero 'price' (no target) and severely outlier 'price'.  
* Drop rows with no 'odometer' values.  
* Drop rows with no 'year' values.  
* Drop rows where 'cylinder' is not 4, 6, 8.  
  
#### NaN  
* There is significant number of NaN entries, especially in the following features: 'condition', 'cylinders', 'drive', 'size', 'type', 'paint_color'. It seems that dropping all the NaN data rows will affect the data size, but for simplification, I have chosen to drop rows having >3 NaNs. Further actions of NaN rows can be used to optimize compute time.
* Columns that have more than a third of the data as NaN can also be removed for simplification
  
#### Improving focus by removing some categories and reducing cardinality   
* The data for 'year' is too widespread. Obviously 1950 automobiles are not going to impact the price or sellability of more recent cars. A fair approximation is to remove all data that have 'year' < 2001.
* 'model' has 29000+ unique values. Models with < 200 entries can be removed to improve any outliers.
* Similarly 'manufacturer' with less than 5000 data rows can be dropped to avoid noise in the regression.  
* Map states to geographic regions based on us census data
  
#### Column Data Observation    
* 'year' and 'odometer' have numerical data. The rest of the columns have categorical data and will need One Hot Encoding (OHE).    
* 'model' and 'odometer' have high unique data. These may need clustering before OHE.  
* The 'cylinders' column maybe converted to numerical catergory by mapping values.  
  
#### Data Preparation based on initial regression runs  
* In the initial iterations of the work, I filled the NaN data points with "notnumber" However, dataset was too big for my computer with 10-24+ hours of run time for the grid search steps So I chose to remove the NaN rows - it did cause lose of datapoints But, I believe the methods and code is valid. This will be a future work.  
* The earlier iterations had indicated that paint_color is not a critical feature. So first drop that column and then drop the rows with NaN
  
## Modeling

## Evaluation

## Deployment
The Jupyter Notebook for this assignment is checked into the Git Hub repository at https://github.com/rvraj26/Assignmen11.1_RajVarada
  
## Results

## Future Work
1. Enhance the code to identify the features are a good predictor of a used car sale, given the attributes of a customer and an automobile
2. Enhance the NaN handling. Have to add a method to add default values for missing data. Add more methods to reduce the amount of data removed due to NaN 
3. Address and remove the simplifying assumptions during data preparation  
