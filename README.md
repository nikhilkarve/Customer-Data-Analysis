# Customer-Segmentation.

Performing data analysis and customer segmentation by dividing a store's customers into groups that reflect similarity among customers in each group. The goal of segmenting customers is to decide how to relate to customers in each segment in order to maximize the value of each customer to the business.

## Business Understanding.

Retail business is selling consumers products/services for their personal use. This business happens through different ways such as in-store, online etc. Dataset we are using here is of an online store.

## Business Problem.

There are some important questions, answers of which may increase the profit of the business.
- Number of products sold every month.
- Amount of money customers spend every month.
- Performing customer segmentation analysis.
- Recommendations based on segmentation analysis.

## Dataset.
[Source](https://archive.ics.uci.edu/ml/datasets/online+retail). 

## Data preparation and cleaning.
- Observation 1: CustomerID has around 25% of missing values. For 0.26% of items 'Description' seems to be missing. As we can't fill up 25% missing values of CustomerID we need to remove them.
- Observation 2: In Data Quality Report of numerical variables, you can see 'Quantity' has a minimum value of -80995.00 and maximum value of 80995. This must be returned/cancelled orders. Even 'UnitPrice' also has a negative value of -11062.06 which is unusual. This also must be representing cancelled/returned orders. Hence, removing negative values from 'Quantity' and 'UnitPrice'.
- Observation 3: As you can see in 'Country' row, 91.43% of the orders have 'United Kingdom' as country. Hence only keep values belonging to 'United Kingdom' as 'Country'.
- Packages: Pandas, Numpy, Matplotlib, Seaborn, Sklearn.

## Exploratory Data Analysis.
Number of products sold every month.
![alt Number of products sold every month.](https://github.com/nikhilkarve/Customer-Data-Analysis/blob/main/viz/products.jpeg)

Amount of money spent each month? 
![alt How much customer spend their money every month?](https://github.com/nikhilkarve/Customer-Data-Analysis/blob/main/viz/spending.jpeg)

Revenue in November seems highest. This is an example of a critical information business can use. They can increase the advertisement of new products that month to promote them and increase their sell.

## RFM Analysis.
What Is Recency, Frequency, Monetary Value (RFM)?
Recency, frequency, monetary value is a marketing analysis tool used to identify a company's or an organization's best customers by measuring and analyzing spending habits.

An RFM analysis evaluates clients and customers by scoring them in three categories: how recently they've made a purchase, how often they buy, and the size of their purchases.

The RFM model is based on three quantitative factors :-

1. Recency: How recently a customer has made a purchase.
2. Frequency: How often a customer makes a purchase.
3. Monetary Value: How much money a customer spends on purchases.

[Source](https://www.investopedia.com/terms/r/rfm-recency-frequency-monetary-value.asp)

## Modeling Data for RFM Analysis.
RFM Data
- Dividing the RFM data in quantiles and assigning values to the customers between 1 to 4 for all the three sections by comparing values to the quantiles. Here 4 will be the highest and 1 will be the lowest.
- 
![alt](https://github.com/nikhilkarve/Customer-Data-Analysis/blob/main/dataframes/rf_model.jpeg)

- Champions are your best customers, who bought most recently, most often, and are heavy spenders. Reward these customers. They can become early adopters for new products and will help promote your brand.
- Potential Loyalists are your recent customers with average frequency and who spent a good amount. Offer membership or loyalty programs or recommend related products to upsell them and help them become your Loyalists or Champions.
- New Customers are your customers who have a high overall RFM score but are not frequent shoppers. Start building relationships with these customers by providing onboarding support and special offers to increase their visits.
- At Risk Customers are your customers who purchased often and spent big amounts, but haven’t purchased recently. Send them personalized reactivation campaigns to reconnect, and offer renewals and helpful products to encourage another purchase.
- Can’t Lose Them are customers who used to visit and purchase quite often, but haven’t been visiting recently. Bring them back with relevant promotions, and run surveys to find out what went wrong and avoid losing them to a competitor.

[source](https://clevertap.com/blog/rfm-analysis/)
![alt](https://github.com/nikhilkarve/Customer-Data-Analysis/blob/main/viz/cust_type.jpeg)

## Using K-Means

We know that K-means clustering algorithm is sensitive to outliers. There are outliers present in our data.

- How to identify outliers?

One way of identifying outliers is to compare the gaps between the median, minimum, maximum, 1st quartile, and 3rd quartile values. If the gap between the 3rd quartile and the maximum value is noticeably larger than the gap between the median and the 3rd quartile, this suggests that the maximum value is unusual and is likely to be an outlier. Similarly, if the gap between the 1st quartile and the minimum value is noticeably larger than the gap between the median and the 1st quartile, this suggests that the minimum value is unusual and is likely to be an outlier. Mathematically, multiplying the interquartile range (IQR) by 1.5 will give us a way to determine whether a certain value is an outlier. If we subtract 1.5 x IQR from the first quartile (lower threshold), any data values that are less than this number are considered outliers. Similarly, if we add 1.5 x IQR to the third quartile (upper threshold), any data values that are greater than this number are considered outliers.

- How to deal with outliers here?

To deal with these outliers we can use clamp transforamtion. In this we convert the values which are lower than the lower threshold to lower threshold and values which are greater than the upper threshold to upper threshold.

Later, I have used log-transformation to reduce the skewness of the data. After log transformation, I have performed standardization.

- Finding optimal K

Using Silhoutte score.
![alt](https://github.com/nikhilkarve/Customer-Data-Analysis/blob/main/viz/k-silh.jpeg)

Using Davies-Bouldin
![alt](https://github.com/nikhilkarve/Customer-Data-Analysis/blob/main/viz/davscore.jpeg)

We found that 4 is the optimal value of K here.

## Interpretation of the clusters formed.
![alt](https://github.com/nikhilkarve/Customer-Data-Analysis/blob/main/viz/final_clusters.png)

- We can interpret these clusters as follows:

Cluster 2 has the lowest 'Recency' value, highest 'Frequency' value and highest 'Monetary' value. Hence, we can safely say that the Cluster 3 represents 'Champion' customers. (Those who buys frequently, spend good amount and are amoong recent buyers). This comprises around 24% of our data. Reward these customers. They can become early adopters for new products and will help promote your brand.

Cluster 1 has the highest 'Recency' value, lowest 'Frequency' value and lowest 'Monetary' value. Hence, we can safely say that the Cluster 1 represents 'Lost Customers', who were never frequent buyers or big spenders.

Cluster 0 has high 'Recency' value, but it also has high 'Frequency' and 'Monetary' value. Hence, we can say that th Cluster 0 represents 'At Risk' customers. At Risk Customers are your customers who purchased often and spent big amounts, but haven’t purchased recently. Send them personalized reactivation campaigns to reconnect, and offer renewals and helpful products to encourage another purchase.

Cluster 3 has comparitively low overall RFM score but they have bought frequently. They may reprsent 'Almost lost' customers or some of the points may be representing 'New customers' as they have bought products recently.
