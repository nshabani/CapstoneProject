# Udacity Data Science NanoDegree Capstone Project-Create a Customer Segmentation Report for Arvato Financial Services

In this project, I analyzed demographics data for customers of a mail-order sales company in Germany, compared it against demographics information for the general population. I used unsupervised learning techniques to perform customer segmentation, identified the parts of the population that best describe the core customer base of the company. Then, I applied what I learned on a third dataset with demographics information for targets of a marketing campaign for the company, and used a model to predict which individuals are most likely to convert into becoming customers for the company. The data that I used has been provided by Bertelsmann Arvato Analytics, and represents a real-life data science task.

The data file for the general population contained demographics data of 891,211 records and 366 features. The dataset for customers of the mail-order company contained 191,652 persons and 369 features.

## Data preprocessing

 Since the datasets were pretty large, and at this point I was going to do unsupervised learning without any predictions, I decided to eliminate as much features as possible to make data 
 handling easier. Starting by data wrangling, I went through the excel file containing definitions of the most of the features. I only kept the features with available definitions, since I could not interpret features without definition. There were features related to the same definitions with different levels of categorization (such as CAMEO_DEU_2015 series), I decided to keep only the one which has highest level of categorization with lowest number of categories, and remove the rest. I also replaced some feature values with more meaningful values and summarized some values to reduce number of categories. 

Handling missing values is an important issue. First I replaced the values corresponding to unknown values from the definition file with null. Then I checked the ratio of missing values in rows and columns of both datasets. There were several rows and columns in the population dataset with more than 20% missing values. These rows and columns were deleted. The same columns were also deleted from customers dataset to keep the two tables consistent. There were more columns with missing values in customers dataset that were kept to be imputed later. Rows with more than 20% missing values were deleted from customer dataset too.
By looking at features definitions, I divided them to three groups: numeric, nominal and ordinal features. For the numeric features, I imputed null values by feature’s mean to keep the statistics unchanged. For the rest of the features, I replaced null values with zero. This helps to identify unknown values as a separate category. 
So data preprocessing steps included:
-	Removing and replacing some features and values
-	Removing columns with more than 20% missing values in reference dataset
-	Removing rows with more than 20% missing values
-	Replacing null values
I took a quick look at distribution of 10 randomly pick features over both dataset, with a quick glance I could identify some differences between customers and general population.
The next steps of data preprocessing were:
-	Replacing nominal features with dummy variables. I only did that with features with less than 5 categories to prevent the dataset from being too large to handle. The rest of nominal variables were deleted.
-	I used standard scaler to center and scale the data to be able to use PCA.
 
## Customer segmentation report
The feature dimension of the dataset at this point was down from 366-369 to 289. To see if it can be reduced, I used principal component analysis. The result show that 180 component can explain more than 95% of variance, representing that there is correlation between features. Then, I re-fit a PCA instance with 180 components to perform the decided-on transformation.
I then checked the components to see if I can interpret them from feature weights. 
The interpretation was not very straightforward since the feature definitions were not very clear, however, for instance I could say:
- Component 1 was related to high income, business units with high share of car.
- Component 2 was related of share of BMW and Mercedes
- Component 3 was related to share of different types of cars
- Component 4 was related to transactions 
- Component 5 was related to how traditional, cultural and religious minded a person is 

In the next step, K-mean method was used for clustering azdias dataset. First, I used different number of clusters and checked sum of squared error. Having them in a elbow plot enabled me to pick appropriate K, which showed that 8 cluster is sufficient. The K-mean model with K=8 was fit to general population dataset and was used to make prediction of the cluster labels. Then the same model was used to transform and predict clusters for customers dataset. I then calculated the proportions of each cluster for the population dataset. I also include the data that was initially removed due to too many nulls when calculating the proportions and include this data as cluster -1.
The same process was applied to customers dataset. And finally I compared the distribution of the frequencies over the clusters for population and customers dataset as was shown in graphs below. It showed that people in clusters 1 and 5 are more likely to be customers of the company. If we look at the components with the highest and lowest weights for these two clusters, we could see in both clusters, component 1 had the highest weight. As mentioned before, component 1 looked to be more related high income, business related people. So maybe focusing on these groups and looking more closely to component 1 can provide insights on potential customers.
## Part 2: Supervised Learning Model

Now that we found which parts of the population are more likely to be customers of the mail-order company, it was time to build a prediction model. Each of the rows in the "MAILOUT" data files represents an individual that was targeted for a mailout campaign. Ideally, we should be able to use the demographic information from each individual to decide whether or not it will be worth it to include that person in the campaign.

The "MAILOUT" data has been split into two approximately equal parts, each with almost 43 000 data rows. In this part, we can verify your model with the "TRAIN" partition, which includes a column, "RESPONSE", that states whether or not a person became a customer of the company following the campaign. In the next part, you'll need to create predictions on the "TEST" partition, where the "RESPONSE" column has been withheld.
The same data preprocessing as above was done to the mailout_train dataset except  no column were deleted due to having missing values. The reason is that the column with the highest missing values had 22% null values so I decided to keep as much information as possible. 
I fit different types of classifier to the training set to check the runtime and area under the curve for comparison including Logistic Regression, Random Forest, Ada Boost, Gradient Boosting and Support Vector Classifier. The result showed that Gradient Boosting method had slightly higher area under the curve than Ada boost but it took longer to run. Also, while area under the curve was not very high for Random Forest model, it ran very fast. I picked Ada Boost and Random Forest model for further analysis and tried to improve them using hyperparameter tuning. With all the changes I tried, still the base case Gradient Boosting model provided higher area under the curve. So I decided to use this model as classifier. 
Finally, I performed data preprocessing to the test set, without deleting columns and rows with missing values since I need to predict values for all the existing rows. The result was saved in a format that can be used in a Kaggle competition.
Areas for improvement:
-	For imputing null values, more advance models can be used. Instead of replacing missing values with mean, median or zero, we can use predicting model based on the columns with no missing values.
-	Spending more time on clarifying feature definition is worth giving a shut, since we might be able to do better interpretation in clustering part.
-	To improve the prediction model, more time can be spent exploring different models with more hyperparameter tuning to reach higher AUC. 

