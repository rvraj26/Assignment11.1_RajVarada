# UCB ML/AI Certification: Practical Assignment 11.1
## Problem Statement

The problem is to find features in a dataset that determines the price and sellability of used cars.  
The goal of the assignment is to find the factors that determine car price and to provide clear recommendations to the client -- a used car dealership -- as to what consumers value in a used car.
  
## Methodology  

The CRISP-DM Methodology was used for this exercise. In the Data Understanding and Data Preparation steps, several insights were gained, which was used for feature selection. Based on the data analysis, a few features that would not impact the results were dropped, and same with the outlier entries. The data was also cleaned up to remove outliers, and some columns with high cardinality were reduced to a lower cardinality. The data was then preprocessed to address missing values, encoding categorical variables, and scaling numerical features before finally splitting data into training and test sets. Next in the model building, two linear models - linear and ridge regression and one non-linear model - decision tree was used. The model is evaluated usin mean square error (MSE). 

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

Two linear models - Linear Regression and Ridge Regression, and one non-linear model decision tree were implemented.
  
##### Linear Regression
In the Linear Regression model, I implemented a pipeline with sequential feature selection followed by linear regression. I attempted Polynomial features but the results went haywire. I ran out of time to fully debug it and so and I disable it. The linear model was run with grid search for and selecting between 2-6 features. Perform K fold cross validation with k=4. The optimal grid paramaters and the MSE with optimal paramaters for train and test data set was computed. The best linear features and their coefficients were computed.
  
##### Ridge Regression
The Ridge regression model was implemented same as the Linear Regression model. I initially tried alpha=[0.01, 0.1, 1, 10, 100, 1000], but the run did not converge in my laptop in one day. So I trimmed it to alpha=[0.01, 0.1, 1, 10]. The optimal grid paramaters and the MSE with optimal paramaters for train and test data set was computed. The best ridge features and their coefficients were computed.

##### Decision Tree

and Lasso Regression. Two non-linear models were created: Random Forest and XGBoost.
All 3 linear models performed similarly and were only 63% accurate, based on the R² score of 0.63. Test RMSE for all 3 models was 8845.
GridSearchCV was attempted for the linear models. This required a very long compute time, and only the Ridge model parameter of Alpha = 10 was obtained. Attempts to get other best parameters did not work as GidSearchCV did not converge with the allotted compute power after several iteration increases.
Decision Tree model performed much better than the Linear Models, capturing non-linear relationships in the data, with 82% accuracy based on the R² score of 0.82. Test RMSE was also slightly better = 6188
Random Forest model performed the best by reducing overfitting and averaging multiple trees with 89% accuracy. Test RMSE was also slightly better than Decision Tree, and almost half the value of the RMSE error of the Linear Models. Test RMSE = 4863.
XGBoost performed similarly to Decision Tree with R² score of 0.82 and Test RMSE = 6224

## Evaluation

## Deployment
The Jupyter Notebook for this assignment is checked into the Git Hub repository at https://github.com/rvraj26/Assignmen11.1_RajVarada
  
## Results / Key Findings

## Future Work
1. Enhance the code to identify the features are a good predictor of a used car sale, given the attributes of a customer and an automobile
2. Enhance the NaN handling. Have to add a method to add default values for missing data. Add more methods to reduce the amount of data removed due to NaN 
3. Address and remove the simplifying assumptions during data preparation  
