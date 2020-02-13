# instacart17 : Predict-next-order-of-users
Instacart Market Basket Analysis competition hosted on Kaggle, team project, Sep 2017

## Task Description
This task is reformulated as a binary classification problem: given a user, a target product, and the user's purchase history, predict whether the target product will appear in the user's next order. The evaluation metric is the F1-score between the set of predicted products and the set of true products.

## Data Description
The dataset is an open-source dataset provided by Instacart (https://www.kaggle.com/c/instacart-market-basket-analysis):

This anonymized dataset contains a sample of over 3 million grocery orders from more than 200,000 Instacart users. For each user, we provide between 4 and 100 of their orders, with the sequence of products purchased in each order. We also provide the week and hour of day the order was placed, and a relative measure of time between orders.

## Below is the full data information:

orders (3.4m rows, 206k users):

order_id: order identifier
user_id: customer identifier
eval_set: which evaluation set this order belongs in (see SET described below)
order_number: the order sequence number for this user (1 = first, n = nth)
order_dow: the day of the week the order was placed on
order_hour_of_day: the hour of the day the order was placed on
days_since_prior: days since the last order, capped at 30 (with NAs for order_number = 1)
products (50k rows):

product_id: product identifier
product_name: name of the product
aisle_id: foreign key
department_id: foreign key
aisles (134 rows):

aisle_id: aisle identifier
aisle: the name of the aisle
deptartments (21 rows):

department_id: department identifier
department: the name of the department
order_products__SET (30m+ rows):

order_id: foreign key
product_id: foreign key
add_to_cart_order: order in which each product was added to cart
reordered: 1 if this product has been ordered by this user in the past, 0 otherwise
where SET is one of the following evaluation sets (eval_set column in orders):

"prior": orders prior to that users most recent order (~3.2m orders)
"train": training data supplied to participants (~131k orders)
"test": test data reserved for machine learning competitions (~75k orders)


# Approach
## Data Exploratory Analysis

### preprocessing.ipynb 
Navigate the raw data and cleansing 

### FeatureEng_soul.ipynb
#### feature names 
prd_cnt : the number of each items that has been ordered so far

us_prd_cnt : the number of items has the user ordered so far
us_prd_rate : the percentage of each item accounting for over the total order 
us_prd_longago : how many orders have been passed from the last order of each item, for each users. The longer she hasn't bought the same item while placing other orders, the bigger the value is
us_prd_constancy : constancy of a users order to a certain product after the first purchase. The more frequent the order, the higher the value is
  *if a user bought a banana for the first time in her second order, and bought banana for three times in total among her first ten orders, the constancy rate would be: 3/10-2+1 = 1/3)
order_number : check the correlation between the order of each order and re-order rate 
total_distinct_items : number of distinct items each user has ordered in total
order_hour_of_day : the time of the day when the order was placed, grouped into three 
  *assumption: items ordered in certain time of the day(e.g. after work, during lunch time) will have a higher re-order rate
  *processed via R statistics package and later merged into .py 
royalUsers : check the correlation between the time of join and re-order rate
top_prd : Top 10 products with high re-order rates 

### soul0914.ipynb 
This was a team project of four and this file contains my contributions, including trials and errors

### royalusers.ipynb
Merging royal users and checking the correlation between royal users and re-order rate.

### instamodel.ipynb

I used SCV in sklearn.svm for the regression. and tried tree classification (source:https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html)  Optimized F-1. 


