# Telecom Churn Case Study

## Problem Statement

## Business Problem Overview

In the telecom industry, customers are able to choose from multiple service providers and actively switch from one operator to another. In this highly competitive market, the telecommunications industry experiences an average of 15-25% annual churn rate. Given the fact that it costs 5-10 times more to acquire a new customer than to retain an existing one, customer retention has now become even more important than customer acquisition.

For many incumbent operators, retaining high profitable customers is the number one business goal.

To reduce customer churn, telecom companies need to predict which customers are at high risk of churn.

In this project, we will analyse customer-level data of a leading telecom firm, build predictive models to identify customers at high risk of churn and identify the main indicators of churn.

## Understanding and Defining Churn

In the telecom industry, there are two primary payment models: postpaid, where customers receive a monthly or annual bill after using services, and prepaid, where customers pay or recharge a specific amount in advance before utilizing services.

In the postpaid model, when customers decide to switch to another operator, they typically notify their current provider to terminate their services, making it straightforward to identify this as a case of churn.

Conversely, in the prepaid model, customers can easily stop using services without any prior notice when they switch networks. This makes it challenging to determine whether a customer has genuinely churned or is simply not using the services temporarily—such as when someone is traveling abroad for a month or two and plans to resume their usage later.

As a result, churn prediction is particularly critical and complex for prepaid customers, necessitating a careful definition of the term "churn." Additionally, the prepaid model is the most prevalent in India and Southeast Asia, while the postpaid model is more common in Europe and North America.

This project focuses on the Indian and Southeast Asian markets.

## Definitions of Churn 

Churn can be defined in several ways, including:

Revenue-based churn: This refers to customers who have not utilized any revenue-generating services, such as mobile internet, outgoing calls, or SMS, over a specified period. An alternative approach could involve using aggregate metrics, such as identifying customers who have generated less than INR 4 in total, average, or median revenue per month.

However, a significant limitation of this definition is that it overlooks customers who primarily receive calls or messages from wage-earning individuals, meaning they do not generate revenue but still use the services. For instance, many users in rural areas may only receive calls from their siblings working in urban areas.

Usage-based churn: This definition focuses on customers who have not engaged in any usage—whether incoming or outgoing—in terms of calls, internet, etc., over a certain timeframe.

A potential drawback of this definition is that if a customer has ceased using services for a while, it may be too late to implement any corrective measures to retain them. For example, if churn is defined based on a "two-month zero usage" period, predicting churn could be ineffective, as the customer may have already switched to another operator by that time.

In this project, we will adopt the usage-based definition to define churn.
## High-value Churn

In the Indian and the southeast Asian market, approximately 80% of revenue comes from the top 20% customers (called high-value customers). Thus, if we can reduce churn of the high-value customers, we will be able to reduce significant revenue leakage.

In this project, we will define high-value customers based on a certain metric (mentioned later below) and predict churn only on high-value customers.

## Understanding the Business Objective and the Data

The dataset contains customer-level information for a span of four consecutive months - June, July, August and September. The months are encoded as 6, 7, 8 and 9, respectively. 

The business objective is to predict the churn in the last (i.e. the ninth) month using the data (features) from the first three months. To do this task well, understanding the typical customer behaviour during churn will be helpful.

## Understanding Customer Behaviour During Churn

Customers usually do not decide to switch to another competitor instantly, but rather over a period of time (this is especially applicable to high-value customers). In churn prediction, we assume that there are three phases of customer lifecycle :

1. **The ‘good’ phase:** In this phase, the customer is happy with the service and behaves as usual.

2. **The ‘action’ phase:** The customer experience starts to sore in this phase, for e.g. he/she gets a compelling offer from a  competitor, faces unjust charges, becomes unhappy with service quality etc. In this phase, the customer usually shows different behaviour than the ‘good’ months. Also, it is crucial to identify high-churn-risk customers in this phase, since some corrective actions can be taken at this point (such as matching the competitor’s offer/improving the service quality etc.)

3. **The ‘churn’ phase:** In this phase, the customer is said to have churned. We define churn based on this phase. Also, it is important to note that at the time of prediction (i.e. the action months), this data is not available to us for prediction. Thus, after tagging churn as 1/0 based on this phase, we discard all data corresponding to this phase.

In this case, since we are working over a four-month window, the first two months are the ‘good’ phase, the third month is the ‘action’ phase, while the fourth month is the ‘churn’ phase.

## Dataset and Data Dictionary

The dataset can be downloaded from [here](https://drive.google.com/file/d/1SWnADIda31mVFevFcfkGtcgBHTKKI94J/view).

Data dictionary is uploaded. The data dictionary contains meanings of abbreviations. Some frequent ones are loc (local), IC (incoming), OG (outgoing), T2T (telecom operator to telecom operator), T2O (telecom operator to another operator), RECH (recharge) etc.

The attributes containing 6, 7, 8, 9 as suffixes imply that those correspond to the months 6, 7, 8, 9 respectively.

## Data Preparation

The following data preparation steps are crucial for this problem:

1. **Derive new features**
This is one of the most important parts of data preparation since good features are often the differentiators between good and bad models. We will use our business understanding to derive features that we think could be important indicators of churn.

2. **Filter high-value customers**
As mentioned above, we need to predict churn only for the high-value customers. Define high-value customers as follows: Those who have recharged with an amount more than or equal to X, where X is the 70th percentile of the average recharge amount in the first two months (the good phase).

3. **Tag churners and remove attributes of the churn phase**
Now tag the churned customers (churn=1, else 0) based on the fourth month as follows: Those who have not made any calls (either incoming or outgoing) AND have not used mobile internet even once in the churn phase. The attributes we need to use to tag churners are:

- total_ic_mou_9
- total_og_mou_9
- vol_2g_mb_9
- vol_3g_mb_9

After tagging churners, we need to remove all the attributes corresponding to the churn phase (all attributes having ‘ _9’, etc. in their names).

## Modelling

We will develop models to predict customer churn, with two primary objectives:

To forecast whether a high-value customer is likely to churn in the near future (the churn phase). This insight will enable the company to take proactive measures, such as offering special plans or discounts on recharges.

To identify key variables that serve as strong predictors of churn. Understanding these variables can shed light on why customers choose to switch to other networks.

While both objectives can sometimes be achieved with a single machine learning model, the presence of a large number of attributes in our dataset suggests that we should first apply a dimensionality reduction technique, such as Principal Component Analysis (PCA), before building a predictive model. After PCA, we can utilize any classification model.

Additionally, since churn rates are typically low (around 5-10%), we will need to address the issue of class imbalance in our modeling approach.

Here are the suggested steps to build the model:

1.Data Preprocessing: Convert columns to the appropriate formats and handle any missing values.
2.Exploratory Data Analysis (EDA): Conduct thorough exploratory analysis to extract valuable insights, whether directly beneficial for the business or useful for subsequent modeling and feature engineering.
3.Feature Engineering: Derive new features that may enhance the predictive power of the model.
4.Dimensionality Reduction: Use PCA to reduce the number of variables.
5.Model Training: Train a variety of models and tune their hyperparameters, employing techniques to address class imbalance.
6.Model Evaluation: Evaluate the models using suitable metrics, prioritizing the identification of churners over non-churners. Select an evaluation metric that aligns with this business goal.
7.Model Selection: Choose the best-performing model based on the evaluation metrics.

The model developed through these steps will primarily focus on predicting which customers are likely to churn. However, it will not effectively identify the important features associated with churn, as PCA generates components that are often difficult to interpret.

To address this, we will build a second model specifically aimed at identifying significant predictor attributes that can help the business understand the indicators of churn. A logistic regression model or a model from the tree family would be suitable for this purpose. If we opt for logistic regression, we will ensure that we address any issues of multicollinearity.

Once we have identified the important predictors, we will present them visually using plots, summary tables, or any other format that effectively communicates the significance of these features.

Finally, we will recommend strategies to manage customer churn based on our findings.
