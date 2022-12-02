# Rossmann_Sales_Forecasting


 
Problem Statement:

In today’s world, one of the most important factors that Retailers place high importance on is forecasting sales to predict their revenue. Forecasting revenue is often the starting point for preparing an annual budget, which is still challenging for many companies. By forecasting sales, decision makers can plan for business expansion and determine how to fuel the company’s growth. Overall, it helps in business planning, budgeting, and efficiently allocating resources for future growth. 

## Solution:

In this project, a Sales forecasting model will be developed to estimate future sales at each store. By predicting sales, an organization can lay the foundation for many other essential business assumptions and also, sales teams achieve their goals by identifying early warning signals in their sales pipeline and course-correct before it’s too late. 

The organization currently uses a traditional manual system where salespeople prepare their own forecasts by reviewing current accounts and overall projected sales. This approach requires a substantial amount of time and numerous human resources to create projections. 

Our solution will provide an analytical approach to the current process by building a machine learning model that can predict daily sales for up to six weeks in advance without which the trends and patterns across the data such as sales, seasonality, and the impact of other economic factors will be overlooked.

For the Promo2SinceWeek, Promo2SinceYear and PromoInterval columns, we have filled the NaN values with zeros as promo2 column is 0 which indicates that the store is not participating in any kind of promotion.
For the CompetitionDistance column, we have filled the 3 missing values by taking the mean value from this column.
For the columns CompetitionOpenSinceMonth,CompetitionOpenSinceYear we have filled the missing values with the most frequent value i.e. by using the mode() method.


## Feature Engineering:

* Date Features: Extracted basic features like year, month, day, weekofyear, dayofyear from Date. 
* Sales Features: Created new features using Sales and Customers columns. 
* sales_perCustomer - Sales for each customer on each day 
* Avg_sales_per_store - Average sales for each store 
* Avg_customers_per_store - Average customers for each store
* Sales_percustomers_perstore - Average Sales for each customer and store on each day. 
* Holiday features: Calculated the number of holidays in a particular week for StateHoliday and SchoolHolidays. Finally, we found the correlation between all the features using df.corr(). Sales column is highly correlated with avg_customers_per_store, Promo, DayOfWeek, avg_sales_per_store, Open, Customers.


## Data Preprocessing:

* One hot encoding: We have three categorical features (Store Type, Assortment, StateHoliday) in our dataset which describes the store, assortment and categories in StateHoliday as machine learning models require all input and output variables to be numeric. So, we have converted the above 3 features into numeric features using One hot encoding. 

* Trigonometric transformations: We have time-based features such as day of week, day of year, week of year, month etc in our dataset. These features are cyclic in nature and the elegant solution to encode these cyclic features can be using mathematical formulation and trigonometry (sine, cosine) which effectively captures the cyclic nature of these features. 

* Scaling: Scaling is required to rescale the data and it's used when we want features to be compared on the same scale for our algorithm. And, when all features are in the same scale, it also helps algorithms to understand the relative relationship better. We have scaled below numeric features using StandardScaler(). 

## Model Building:

After analyzing the data, it is now time to model the data accordingly and predict the demand. The modeling process involves splitting the data into train and test, then building the model and evaluating its performance This will be performed using four different algorithms. We used supervised machine learning algorithms like Linear Regression, Decision Tree, Random Forest Regression and XGBoost to predict the Sales column. We are splitting the data into training and testing using Time Series split by considering recent data as our test dataset. 

## Model Building: Linear Regression
A basic linear regression model performs poorly on the testing dataset because it fails to find linearity between the target variables and predictors. The model evaluation is performed using the MAE and the RMSPE criteria. The model has a MAE of 813.58 and RMSPE of 2.3 .

As linear regression doesn’t capture the trend, seasonality and other parameters of time we are going to try different models like Decision tree,  Random Forest and Gradient boosting.


## Model Building: Decision Tree

Decision Tree with 10 max_depth gives MAE of 770.94 and RMSPE of 1.85. MAE and RMSPE are reduced when compared to Linear regression models. To get better results we performed hyperparameter tuning using GridSearchCV() which tunes the model using different max_depth: [2,4,6,8,10,12,15,20,30].
Max_depth of 20 achieved the best results. MAE and RMSPE with max_depth of 20 on test data are 651.09 and 1.53 respectively.

## Model Building: RandomForest Regressor

Initially the model is built with n_estimators of 100 which represents the model will be trained on 100 decision trees. RF model achieved MAE of 531.59 and RMSPE of 1.16. There is a drastic decrease in the error when compared to Linear Regression and Decision Tree. RF model yields the following results for variable importance. The top five most important variables were the Open, Sales per customer, day of the week_cos, day of the week_sin.

## Model Building: XGBoost Regressor

Gradient Boosting Tree, like other boosting methods, creates decision trees. Gradient Boosting Tree’s primary notion is to generate the tree as an optimization procedure on an appropriate cost function. Built a basic XGboost model with n_estimators 200 has achieved MAE of 497.99 and RMSPE of 1.15. To get better results we performed hyperparameter tuning using GridSearchCV() which tunes the model using different parameters and best results are achieved with n_estimators=1000, learning_rate=0.2, max_depth=10, subsample=0.9, colsample_bytree=0.7

The XGboost model yields the following results for variable importance. The top five most important variables were the StateHolidays, Sales_perCustomer, Dayofweek_cos. MAE and RMSPE on test data is 450.41 and 1.054 respectively.

## Comparison of Models:

Evaluation metric: We choose Root Mean Squared Error (RMSE) and  Root Mean Squared Percentage error (RMSPE) as the evaluation metrics for our machine learning model. We aim to keep the error between the actual value and the predicted value of the target variable (Sales) minimum. We choose ‘RMSPE’ as it is most useful when large errors are particularly undesirable.


The MAE and RMSPE error measurements were used to calculate the model’s overall performance scores. The study found that the XGboost algorithm produces more accurate forecasts than the random forest, Decision tree and linear regression models. Because the XGboost algorithm learns from the error produced from the previous model. The XGboost technique is easier to use than the random forest model and is less time-consuming. 

## Limitations:
 * Handling large amounts of sales data (10,17,209 observations on 13 variables) was challenging due to high latency in model execution.
 * Around 180 stores were closed for 6 months. Unable to fill the gap of sales for those stores. 
 * Prediction of sales for individual stores(out of 1115) and most stores have different patterns of sales. A single model cannot justify all stores.  

## Conclusion:
To conclude, the XGBoost technique was the most accurate because of its ability to master real-time dynamics and improve pattern identification and prediction from the current window. It is pretty computationally tricky; however, with the right amount of training data, it will be much more accurate and an excellent forecast to estimate demand and plan for resources in advance. 
