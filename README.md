# Udacity Data Science NanoDegree Capstone Project-Create a Customer Segmentation Report for Arvato Financial Services

## Problem Statement
In this project, the demographics data for customers of a mail-order sales company as well as demographics data of general population in Germany are provided. The objective is to compare the two dataset using unsupervised learning techniques to perform customer segmentation, and to identify the parts of the population that best describe the core customer base of the company. Then, apply what is learnt on a third dataset with demographics information for targets of a marketing campaign for the company, and use a model to predict which individuals are most likely to convert into becoming customers for the company.
The data has been provided by Udacity partners at Bertelsmann Arvato Analytics, and represents a real-life data science task.

##Methodology
My strategy to solve the problem includes following steps:
- Data exploration to understand input data, definition of features and indicate their types (numeric vs categorical).
- Find appropriate method to impute missing values and perform data preprocessing and feature engineering
- Customer segmentation using unsupervised learning
- Develop predicting models based on the training set, define metrics for choosing between different methods, apply model to the test set, improve model, and compare results.

Data preprocessing steps included:
- Removing and replacing some features and values
- Removing columns with more than 20% missing values in reference dataset
- Removing rows with more than 20% missing values
- Replacing null values by feature’s mean for numeric features and zero for rest of the features
- Replacing nominal features with dummy variables. 
- Applying standard scaler to center and scale the data to be able to use PCA.

For unsupervised learning, the K-mean model with K=20 was fit to general population dataset and was used to make prediction of the cluster labels. Then the same model was used to transform and predict clusters for customers dataset. Then the proportions of each cluster for the population dataset was calculated.
For supervised learning, different methods were tried, The results showed that Gradient Boosting method had slightly higher area under the curve than others. More analysis can be done to improve model performance.

## Results
In summary the following points were found from this analysis:
1- Top earner high income people who are home owners with few families in their building and low mobility should be main group to be targeted in marketing campaigns. This group were found to have high weight in three out of four clusters with highest ratios of customers.
2- Another group is people with professional and/or academic titles. Although this group is small in the population, they have highest ratio of customers among all identified clusters.


The full discussion and explanation for the project can be found [here](https://medium.com/@nazanin.shaebani/who-is-likely-to-be-our-customer-2586e266b7f1).
Data prepe
