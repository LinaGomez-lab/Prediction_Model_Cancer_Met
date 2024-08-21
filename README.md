# Prediction-Model---Catboost

## **I. Project Description**

### **Context:**

This project is part of the Women in Data Science (WiDS) Datathon 2024 (Challenge 2), focusing on a healthcare issue crucial for advancing equity for women globally. The datathon aims to identify potential disparities in treatment and investigate the underlying factors contributing to these biases, including demographic and societal elements. Addressing healthcare inequity is a global challenge with a significant impact on women’s health, which is vital for the well-being of societies and economies.

### **Objective:**
This project proposes a predictive model to estimate the time to metastatic cancer diagnosis in breast cancer patients, identifying key features that influence this timeline.

### **Business Goal:**
"Develop a predictive model proposal for the breast cancer metastatic diagnosis period that is accurate, identifies the main features relevant for healthcare decision-making, and is easy to update over time".

### **Data Description**

The Oncology Dataset was provided by WiDS and Gilead Sciences. It includes information on 18.819 patients, sourced from Health Verity, and is supplemented with third-party geo-demographic data to provide insights into socio-economic factors that may affect health equity. For this challenge, the dataset was further enhanced with zip code-level climate data, comprising a total of 152 variables.

### **Data Dictionary**

* Main variables (16): patient_id, patient_race, payer_type, patient_state, patient_zip3, Region, Division, patient_gender, bmi, breast_cancer_diagnosis_code, breast_cancer_diagnosis_desc, metastatic_cancer_diagnosis_code, metastatic_first_novel_treatment, metastatic_first_novel_treatment_type, metastatic_diagnosis_period.
* Sociodemographic variables(64): Includes average participation by ZIP code (3-digit) segmented by age group, marital status, income levels, education levels, employment statud, race, and other factors.
* Monthly temperatures by ZIP code (72): Average temperatures by ZIP code (3-digit) from January 2013 to December 2018.

For a more detailed data description, follow this link. [Datathon WiDS 2024-Challenge 2](https://www.kaggle.com/competitions/widsdatathon2024-challenge2/data)

### **Data Model Strategy:**

The accuracy of the final proposed model relies on the processes of data cleaning, feature engineering, and model training. Its relevance was determined by leveraging business-related information in healthcare, specifically concerning breast cancer.   

* **Feature pre-selection:**

  - **Literature** recognizes age, race, BMI, and environmental factors like climate change as risk factors for breast cancer. Additional factors, such as payer type and patient state—related to state-level regulatory, legislative, or environmental variables—were considered as main features in the preselection process.

  - During **model training**, Optuna was used to select from other sociodemographic factors related to income, education levels, and barriers to healthcare access, based on accuracy metrics.

* **Data Cleaning:**

  - **Outlier correction (noisy data):** *BMI* contains outlier values that affect its significance in the models. Given the real issue of extreme obesity, these outliers were addressed by replacing them with the variable’s upper bound values. Other data errors on patient states were corrected.

  - **Missing values:** A major challenge in this database is the high proportion of missing data in key variables such as *BMI, patient_race, and payer_type*. To address this, missing values were imputed using the mean for numerical variables and the mode for categorical variables. The imputation was performed sequentially by *patient_zip3, patient_state, and Division*, respectively. The same sequential imputation process was applied to address missing values for other variables. 
  The variables metastatic_first_novel_treatment_type and metastatic_first_novel_treatment were dropped because they contained data for only 13 out of 13,173 observations.

  - **Variety (handling text data):** Variables requiring special treatment include 'breast_cancer_diagnosis_code', which needs to be addressed with different versions of the International Classification of Diseases (ICD9 vs. ICD10), and 'breast_cancer_diagnosis_desc', which involves handling text data for variable descriptions.  


 * **Feature Engineering:**
   
  - **New features:** Climate Change was calculated through a two-step process: Step 1: Calculate the standard deviation of temperatures for each month from 2013 to 2018; and Step 2: Compute the mean of these monthly temperature standard deviations by ZIP code (3-digit). In Step 1, if there were no climate change, we would expect the monthly standard deviations to be zero. In Step 2, the mean is calculated by ZIP code, capturing variations in climate change impact across different areas. The variable **"Climate Change"** has a mean value of 3.1°F, indicating that average temperatures have increased by 3.1°F. The minimum value is 1.3°F, and the maximum value is 4.6°F.

  - **Data Heterogeneity:** Robust Scaling and different Ordinal Encoding techniques were applied. 

* **Model Training:**

  - **Models:** Models trained include 'CatBoost', 'XGBoost', 'LightGBM', 'Gradient Boosting', 'Random Forest', 'XGBRFRegressor', 'Extra Trees Regressor', 'AdaBoost Regression', 'KNeighbors Regressor', 'Ridge', and 'SVR'. To reduce overfitting, 'Stacking' and 'Voting' ensemble techniques were applied. Feature importance analysis was conducted for the top three models. 

  - **Imbalanced data:** A crucial challenge for model performance in this project is managing imbalanced data. The target variable, 'metastatic_diagnosis_period' (in days), is highly concentrated with zero values and exhibits right skewness. Additionally, the distribution of 'breast_cancer_diagnosis_code' influences the distribution of other key feature variables in the dataset, particularly for ICD-9 versus ICD-10 versions. This challenge was addressed in the cross-validation process with a customized StratifiedKFold per group, where the group was created using the 'breast_cancer_diagnosis_code' information.

  - **Hyperparameters and optimization:** In different stages, GridSearchCV or Optuna were used for fine-tuning and optimization processes.
 

## **II: Business Report 📰:**
A dashboard summarizing the main results can be found at the following link:

### [Business Report](https://infograph.venngage.com/pl/5lQsdECOi68)

