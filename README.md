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

The decision tree was implemented and the same set of scores as the linear and ridge models were computed. 

The results are summarized below. 

## Evaluation

##### Best Model

The models above performed well, and the results have interesting similarities and differences. The table below summarizes the results.

![image](https://github.com/user-attachments/assets/da2fa8c9-5a42-499b-9c59-16b935578085)

The recommendation is to use the Ridge Model with Optimal Grid params {'ridge__alpha': 0.1, 'sfs__n_features_to_select': 9}

##### The Number One factor that determines the `price`
All three models unanimously agreed that the `year` of the car is the number one factor determining the `price`. 

Two other features are common among the models: `drive_fwd` and `cylinders_4 cylinders`. However, the coefficients for these are not in the same direction, and this will be examined closely in future work. 

The earlier heat map analysis of the numeric features indicated that the `odometer` feature is an indicator of `price`. This is also the case in my car-buying experiences, with the `price` decreasing with the `odometer`. However, only two of the three models picked this as best features, and only the Ridge Regression model shows the inverse relationship. 

##### Recommendation to client

The recommendation to the client is to use these key drivers and the model to predict the price. Additionally, the client will derive value by analyzing the data by Year of manufacture, subject to secondary features like FWD, 4 cylinders and odometer reading.  

## Deployment
The Jupyter Notebook for this assignment is checked into the Git Hub repository at https://github.com/rvraj26/Assignment11.1_RajVarada


#### Deployment Plan
The model can be deployed quickly. The client should be advised about the training run times, especially for the Ridge Regression model. Adequate computing resources have to be secured before the deployment. 

The client should be advised to collect continuous data, both from their business and reliable sources outside. The car industry is undergoing major transformations, and the used car market is expected to be disrupted. The client should be advised to get the models and recommendations updated once every two quarters and on-demand if there is are any major market shifts (for example, a large number of electric vehicles being dumped on the market by rental car companies at reduced prices). 

## Future Work
1. Enhance the code to identify the features are a good predictor of a used car sale, given the attributes of a customer and an automobile
2. Enhance the NaN handling by the use of better assumptions for missing data.  
3. Address and remove the simplifying assumptions during data preparation.   