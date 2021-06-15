# Homework 09 : A/B Testing

In this session, ...

### 1. Goal : 
  - Find the relationship of products.

### 2. Procedure :
  - You can use dataset from link: https://drive.google.com/file/d/1-0sTk_Iw38K-qNeD4y9RB4GWnV65_mGv/view?usp=sharing
  - Define 
      - 'control group' is 'ซื้อ 1 แก้ว แถม 1' 
      - 'treatment group' is 'ซื้อ 2 แก้วลด 50%'
  - You can see Power BI file from [PBI](https://github.com/Tubsamon/BADS7105-CRM/blob/main/Homework%2009%20-%20AB%20Testing/Group8_Cafe%20Amazon_AB%20Testing_Final.pbix) 
  - In this session, you have to know the statistical parameters such as Z-Score, %CI, P-Value, SE, conversion rate. 
      - P-value
     ```
    P-value = 1 - ABS ( NORM.DIST ( [Z Score], 0, 1, TRUE ) )
    ```
      - Standard Error (SE)
    ```
    standard error = SQRT ( ( [conversion rate %] * ( 1 - [conversion rate %] ) / [count view] ) )
    ```   
      - Z-Score
    ```
    Z Score = 
      VAR ctrl_Conversion = [control group conversion rate %]
      VAR treatment_Conversion = [treatment group conversion rate %]
      VAR ctrl_SE = [control standard error]
      VAR treatment_SE = [treatment standard error]

    RETURN
     (treatment_SE - ctrl_Conversion ) / SQRT ( POWER ( ctrl_SE, 2 ) + POWER ( treatment_SE, 2 ) )
    ```   
       - [CI Table](https://github.com/Tubsamon/BADS7105-CRM/blob/main/Homework%2009%20-%20AB%20Testing/CI%20Table.JPG)
      

### 3. Conclusion : 
  - As a result of the visualization, after comparing both campaigns, customers prefer 'ซื้อ 1 แก้ว แถม 1' more than 'ซื้อ 2 แก้วลด 50%'.
