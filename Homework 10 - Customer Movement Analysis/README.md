# Customer Movement Analysis

In this session, I analyze data from Google BigQuery. After querying raw data with SQL, I can consider customer movement from new customers, reactivated customers, repeat customers, and customer churn.


### 1. Goal : 
  - To analyze the movements of 
customers.

### 2. Procedure :
  2.1 You can use data from [link](https://drive.google.com/file/d/1mr8NgqTqBT9lHrNdhVvGSM_Fpo3rri4h/view?usp=sharing).  
  2.2 Query data by Google Big Query   
  - SQL code for query has shown as below. 
      
      
    ```
       SELECT YEAR_MONTH, 
       COUNT ( CASE 
                     WHEN CUSTOMER_TYPE = "NEW CUSTOMER" THEN 1 
                     END) AS NEW_CUSTOMER, 
       COUNT(CASE 
                     WHEN CUSTOMER_TYPE = "REPEAT CUSTOMER" THEN 1 
                     END) AS REPEAT_CUSTOMER, 
       COUNT(CASE 
                     WHEN CUSTOMER_TYPE = "REACTIVED CUSTOMER" THEN 1 
                    END) AS REACTIVATED_CUSTOMER,  
    -(LAG(COUNT(YEAR_MONTH), 1)OVER (ORDER BY YEAR_MONTH)-
      COUNT(CASE  
      WHEN CUSTOMER_TYPE = "REPEAT CUSTOMER" THEN 1 
                     END)) AS CHURN_CUSTOMER 
    FROM 
       (SELECT CUST_CODE, 
                      YEAR_MONTH, 
                    CASE 
                               WHEN PREVIOUS_MONTH IS NULL THEN "NEW CUSTOMER" 
                               WHEN (CAST_YEAR-P_NUM_YEAR)*12+(CAST_MONTH-P_NUM_MONTH) = 1 THEN "REPEAT CUSTOMER" 
                               WHEN (CAST_YEAR-P_NUM_YEAR)*12+(CAST_MONTH-P_NUM_MONTH) > 1 THEN "REACTIVED CUSTOMER" 
                    END AS CUSTOMER_TYPE 
       FROM 
                    (SELECT CUST_CODE, 
                                   YEAR_MONTH, 
                               CAST(SAFE.SUBSTR(CAST(YEAR_MONTH AS STRING), 1, 4) AS NUMERIC) AS CAST_YEAR, 
                              CAST(SAFE.SUBSTR(CAST(YEAR_MONTH AS STRING), 5, 2) AS NUMERIC) AS CAST_MONTH, 
                              LAG(YEAR_MONTH, 1) OVER (PARTITION BY CUST_CODE 
                                        ORDER BY YEAR_MONTH) AS PREVIOUS_MONTH, 
                              LEAD(YEAR_MONTH, 1) OVER (PARTITION BY CUST_CODE 
                                        ORDER BY YEAR_MONTH) AS NEXT_MONTH, 
                              CAST(SAFE.SUBSTR(CAST(LAG(YEAR_MONTH, 1) OVER (PARTITION BY CUST_CODE 
                                        ORDER BY YEAR_MONTH) AS STRING), 1, 4) AS NUMERIC) AS P_NUM_YEAR, 
                              CAST(SAFE.SUBSTR(CAST(LAG(YEAR_MONTH, 1) OVER (PARTITION BY CUST_CODE 
                                        ORDER BY YEAR_MONTH) AS STRING), 5, 2) AS NUMERIC) AS P_NUM_MONTH, 
                    FROM 
                             (SELECT CUST_CODE, 
                                            SUBSTRING(CAST(SHOP_DATE AS STRING), 1, 6) AS YEAR_MONTH 
                             FROM `myfirstproject-312707.TestHW6.sample_data_table` 
                             WHERE CUST_CODE IS NOT NULL 
                             GROUP BY CUST_CODE, 
                                                 YEAR_MONTH)) 
                             ORDER BY CUST_CODE, 
                                                 YEAR_MONTH) 
    GROUP BY YEAR_MONTH 
    ORDER BY YEAR_MONTH 
    ```
    
  2.3 Run query.  
  2.4 Save result in [CSV](https://github.com/Tubsamon/BADS7105-CRM/blob/main/Homework%2010%20-%20Customer%20Movement%20Analysis/Cust_Movement_Result.csv) format.  
  2.5 Analyze data by use Power BI
      - You can see result from [PBI](https://github.com/Tubsamon/BADS7105-CRM/blob/main/Homework%2010%20-%20Customer%20Movement%20Analysis/customer%20movement%20analysis%20dashboard.pbix).
      
 
### 3. Conclusion : 
  The following is the result of customer analysis.
  
  ![](https://github.com/Tubsamon/BADS7105-CRM/blob/main/Homework%2010%20-%20Customer%20Movement%20Analysis/CMA.JPG)

