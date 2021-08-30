# Customer Segmentation
In this session, i use Google Big Query to create segmentation from a [Supermarket Dataset](https://drive.google.com/file/d/1mr8NgqTqBT9lHrNdhVvGSM_Fpo3rri4h/view?usp=sharing).

### 1. Goal : 
  - Find customer segmentation.

### 2. Procedure :
  - Following the addition of data to Google Big Query and the querying of data (shown as below)
      - Create a cluster for each of the four groups.
      - The clustering method is defined as KMEANS++.
      - Define the Euclidean distance type. As shown in the image below
   
   
  ![](https://github.com/Tubsamon/BADS7105-CRM/blob/main/Homework%2006-%20Customer%20Segmentation/Traning%20options.JPG?raw=true)

   - SQL code for customer segmentation in Google Big Query is provided below.


  ```
  CREATE MODEL
    `myfirstproject-312707.TestHW6.TUBSAMON_CLUSTER_3`


OPTIONS
    (
        model_type = 'kmeans',
        num_clusters = 4,
        kmeans_init_method = 'KMEANS++',
        standardize_features = true,
        distance_type = 'euclidean'
    ) 
    
AS
    (
     SELECT 
        
        A.CUST_CODE, 
        TOTAL_VISIT, 
        TOTAL_SPEND, AVG_SPEND,
        AVG_WEEKLY_VISIT, AVG_WEEKLY_SPEND,   
        NO_VISIT_PER_MONTH, TRANSACTION_PER_MONTH, 
        TICKET_SIZE, 
        ARPU_MONTH

        FROM ( 
                SELECT  CUST_CODE,  
                         
                        
                        COUNT(DISTINCT BASKET_ID) AS TOTAL_VISIT,  
                        SUM(SPEND) AS TOTAL_SPEND,  
                        AVG(SPEND) AS AVG_SPEND,
                        COUNT(DISTINCT BASKET_ID)/COUNT(DISTINCT SHOP_WEEK) AS AVG_WEEKLY_VISIT,  
                        SUM(SPEND)/COUNT(DISTINCT SHOP_WEEK) AS AVG_WEEKLY_SPEND,                        
                                        
                        COUNT(DISTINCT BASKET_ID/30)/COUNT(DISTINCT CUST_CODE) AS NO_VISIT_PER_MONTH,  
                        COUNT(DISTINCT(BASKET_ID))/30 AS TRANSACTION_PER_MONTH,  
                        SUM(SPEND)/COUNT(DISTINCT BASKET_ID) AS TICKET_SIZE,                        
                                       
                        ROUND(SUM(SPEND)/COUNT(DISTINCT(ROUND((SHOP_DATE/100),0))),2) AS ARPU_MONTH,    
                

    FROM `myfirstproject-312707.TestHW6.sample_data_table`
    WHERE CUST_CODE IS NOT NULL
    GROUP BY CUST_CODE , SHOP_WEEK) AS A
    )
```

### 3. Conclusion : 
Following the end of the training model, the results are shown below.

 ![](https://github.com/Tubsamon/BADS7105-CRM/blob/main/Homework%2006-%20Customer%20Segmentation/Cust%20Seg.JPG)


  - The biggest group of customers in Centroid 3 are around 47k members, with an ARPU of $6.55 per month, while the smallest group of members has 4k members and an ARPU of $56,000 per month.
  - Other features, such as average spending per time, average spending per week, ticket size, and transaction per month, give the same result as ARPU.
