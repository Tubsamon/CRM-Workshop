# Homework 08 : Campaign Respond Model

In this session, After training a campaign response model, I found that the model's accuracy is also affected by the dataset. If I choose the close year of the dataset, the accuracy will be impacted. In addition, I must consider parameters for creating the appropriate model by using Correlation Matrix Plots and Pair Plots.


### 1. Goal : 
  - Improve model to get more accuracy.

### 2. Procedure :
  - Delete data in 2011-2012. They are resulted in underrated response rate.
  - Add new CLV parameter.
  - Delete some parameter by considering from Correlation Matrix Plots and Pair Plots.
  - After run model again >> get the better result.
  - You can follow step-by-step from link : https://colab.research.google.com/drive/1hPG0twUNGK63S_2I2ChHb2KzmOjf65BL?usp=sharing
  - In the link has steps as below
      - Importing libraries and datasets
      - Data Preparation
      - Calculating response rate
      - Creating train and test dataset
      - Fixing imbalanced with Undersampling
      - Fixing imbalanced with Oversampling
      - Fixing imbalanced with SMOTE
      - Training models for 2 methods : Logistic Regression and XGBoost 
### 3. Conclusion : By using logistic regression model - undersampled
  - Precision = 0.22, Recall = 0.75, f1-Score = 0.34, Accuracy = 0.69, AUC = 0.78
