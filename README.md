# Big Mart Sales Predictions

Big Mart is a chain of grocery stores in India. There data science team collected data from 10 of its stores across various domains relating to the products that were stores as well as the geographic locations of the stores. We will use that data to predict future sale for Big Mart. This was part of a competition hosted by the [Analytics Vidya](https://datahack.analyticsvidhya.com/contest/practice-problem-big-mart-sales-iii/#ProblemStatement) platform on which my solution generated an root mean square error rate less than 98 percent of the near 38,000 submissions at the time.

## Data
The data points collected by the team at Big Mart for the year 2013 are as given below. The credit to the image goes to Analytics Vidya.
![](/images/data.png)

There are eleven features with Item_Outlet_Sales as the label. The distribution was left-skewed and we decided to use the log value for better convergence. The plots are given below.

| Item_Outlet_Sales | log(Item_Outlet_Sales) |
| ----------- | ----------- |
| ![](/images/sales.png) | ![](/images/log_sales.png)|

There are three features that have NULL values:

1. Item_Outlet_Sales: Our dependent variable. Must be noted that the null values are wholly because we have concatenated the already given train and test datasets. The test dataset does not contain  values for number of sales and is filled with NULLs

2. Item_Weight: Might be an important feature in predicting sales. So it does not make sense to remove the values completely. We do not want to lose out on all this data especially since it is around 20 percent of the total. The best strategy is to impute the NaNs. We know that we have an identifier for each product. The same product must have the same weight. Let us use a pivot to get the weights of each product and replace the NaNs.

3. Outlet_Size: Might be very crucial in predicting our dependent variables. Let us borrow a leaf from Item_Weights book. Since this is a categorical variable, let us check the mode of Outlet Size for each type in Outlet_Type. We can also check the individual splits to make sure Outlet_Type is the right variable using pivot to impute for Outlet_Size

## Feauture Engineering and EDA

Let us  look at each of the features and observe its realationship with the label.
We will also try to convert it to a form that can better predict the label.

List of features and operations:

1. Item weight
   - Impute NaNs by pivoing
   - Convert continuous weights to discrete buckets of weights
2. Outlet Size
    - Impute NaNs by pivoting with Store Size
3. Item Visibility
   - Impute 0 values with average visibility of product type as zero visibility of a product is non-intuitive
4. Item Type
   - Group categories into a broader Item_Category feature that can better predict sales
5. Fat Content
   - Clean up naming errors while data collection
   - Create an extra category for Non-Edible Items
6. Establishment year
   - Convert feature to years since establishment. The lower value can help model converge better.


## Models

The table below summarizes the performance of various models used to predict Sales.

| Type | CV Loss |
| ----------- | ----------- |
| Decision Tree |1578 |
| Linear Regression | 1197  |
|Ridge   |  1129 |
|Lasso   |  1129 |
|Random Forest   | 1088  |
|Gradient Boosting   | 1086  |
## Conclusion

In this project we  used the Big Mart data to predict sales for different outlets given certain features. We tested on many different models and tried to optimize them by trying to tune the hyperparameters using Grid Search techniques. A great chunk of the problem was focussed on EDA and feature engineering. It turns out the Graadient Boosting Tree achieved the best performance with a cross validated RMSE of 1086. It must be noted that the sales ranges from 30 to 14000.

## Future Work

Firstly, we can try training the model on a subset of features by only including those that can better exaplain the dependent variable. Secondly try to engineer more features by compunding data for the geographic locations of the stores as they may map the buying pattern a lot better. 
