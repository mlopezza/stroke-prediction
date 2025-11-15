# Statistical analysis

## Confounding Factors
It was decided to remove all observations under 18 years old to reduce noise associated with the physiological differences between the pediatric and adult populations. This decision was made considering that the causes of stroke in children are mostly related to genetic factors and other non-preventable conditions, leaving no room for preventive interventions, and because the incidence is very low (1.2 to 13 cases per 100,000 children under 18 years old per year).

In addition, there were only two stroke cases under 18 years old; therefore, their removal had no impact on the final outcome and helped increase the accuracy of the model and analysis.

## Variability
Variability was handled through the application of standardized categories:

### BMI Variability:
- **BMI classification from the CDC (4):**
    - Underweight:  <18.5
    - Healthy Weight: 18.5>25 
    - Overweight: 25>=30
    - Obesity: => 30 

### Glucose Average Variability:
During the data analysis, an important limitation was identified: the lack of information regarding the method of glucose measurement. It was unknown at what time or under which conditions the measurements were taken, suggesting that they were likely not performed under ideal fasting conditions (Fasting Plasma Glucose).

To address this uncertainty, the American Diabetes Association (ADA) classification for the Oral Glucose Tolerance Test (OGTT) was adopted. This decision was based on the assumption that patients had likely consumed food during the day at the time of measurement. This approach aimed to ensure that the interpretation of the results was as appropriate as possible given the available data conditions.
 
- **ASA classification (OGTT)(5):**
    - Low glucose level les tan 70 mg/dl
    - Healthy Value 70 yo 140 mg/dl 
    - Prediabetes or oral glucose intolerance. 141 to 199 mg/dl
    - Diabetes: More than 200 mg/dl

### Age Variability:

After excluding the pediatric population, the Canadian age classification was adopted (6). Additionally, the potential impact on youth categories was considered following the exclusion of observations under 18 years of age:

- **Canadian classification for age:**
    - Children 0-14 years old
    - Youth 15-25 years old
    - Adult 25 to 64 years old
    - Senior 65 and more years old


# Feature selection:

A test of independence was conducted to determine which features were significant among stroke patients. The following variables were included in the analysis:

- **Numerical Variables:** age (3 categories), hypertension, heart_disease, avg_glucose_level (4 categories), bmi (4 categories)  
- **Categorical Variables:** gender, ever_married, work_type, residence_type, smoking_status  

To evaluate significance:  
- **T-test** was applied to continuous numerical variables.  
- **Chi-Square test** was applied to categorical variables.  

These results guided the selection of features for building the predictive stroke model.  

#### Logistic Regression Interpretation
- **Positive coefficient:** increases the likelihood (log-odds) of stroke.  
- **Negative coefficient:** decreases the likelihood.  
- **P-value:** indicates statistical significance, with p < 0.05 considered significant.  

#### Results
- **Variables with p-value > 0.05 (not significant):**  
  - gender  
  - glucose category (low)  
  - residence_type  
  - work_type (private, govt_job, never_worked)  

- **Variables with p-value < 0.05 (significant):**  
  - age  
  - glucose category (diabetic, healthy, prediabetic)  
  - hypertension  
  - heart_disease  
  - work_type (self-employed)  

- **Variables requiring deeper analysis:**  
  - ever_married: significant  
  - smoking_status:  
    - significant: formerly smoked  
    - not significant: never smoked, smokes, unknown  
  - bmi category: not significant


# Ever Married Analysis:
### Marital Status and Age-Adjusted Analysis

There were concerns about the true correlation between marital status and stroke, and how age could impact the results. To address this, a test of independence adjusted for age was applied.  

#### Results
- **ever_married_num**: coefficient = -0.139, p = 0.529 (not significant)  
  - After adjusting for age, having ever been married is not independently associated with stroke.  

- **age**: coefficient = 0.0763, p < 0.001 (highly significant)  
  - Each additional year of age increases the log-odds of stroke by 0.0763.  
  - This translates to approximately a 7.9% increase in stroke risk per year of age.  

#### Conclusion
- Given these results, **ever_married** was excluded from the final predictive model.



# Smoking Status
#### Results
- **smoking_status_formerly smoked:** coefficient = 0.1544, p-value = 0.454  
- **smoking_status_never smoked:** coefficient = -0.0516, p-value = 0.792  
- **smoking_status_smokes:** coefficient = 0.2826, p-value = 0.222  

#### Interpretation
- **P-Value:**  
  All p-values are greater than 0.05, indicating that none of the smoking status variables are statistically significant predictors of stroke in this model.  

- **Coefficient:**  
  - **formerly smoked:** Positive coefficient suggests that people who smoked in the past may have slightly higher odds of stroke compared to the reference group, but this effect is not statistically significant.  
  - **smokes:** Positive coefficient suggests that current smokers may have higher odds of stroke compared to the reference group, though not statistically significant.  
  - **never smoked:** Negative coefficient suggests that people who never smoked may have slightly lower odds of stroke compared to the reference group, but the effect is not statistically significant.  

#### Conclusion
Although the effects are not statistically significant in our dataset, the coefficients suggest that individuals who currently smoke or smoked in the past may have slightly higher odds of stroke, while those who never smoked may have slightly lower odds. Considering the relatively low number of stroke observations and in alignment with larger studies that show a correlation between smoking and stroke, smoking status was included as a variable in the final predictive model.


# BMI 
#### Results
- **bmi_category_Obesity:** coefficient = 0.2011, p-value = 0.335  
- **bmi_category_Overweight:** coefficient = 0.1993, p-value = 0.358  
- **bmi_category_Underweight:** coefficient = -1.0194, p-value = 0.331  

#### Interpretation
- **P-Value:**  
  All p-values are greater than 0.05, indicating that none of the BMI category variables are statistically significant predictors of stroke in this model.  

- **Coefficient:**  
  - **Obesity:** Positive coefficient suggests that individuals with obesity may have slightly higher odds of stroke compared to the reference group, but this effect is not statistically significant.  
  - **Overweight:** Positive coefficient suggests that individuals who are overweight may have slightly higher odds of stroke compared to the reference group, though not statistically significant.  
  - **Underweight:** Negative coefficient suggests that individuals who are underweight may have slightly lower odds of stroke compared to the reference group, but the effect is not statistically significant.  

#### Conclusion
Although the effects are not statistically significant in our dataset, the coefficients suggest that individuals with obesity or overweight may have slightly higher odds of stroke, while those who are underweight may have slightly lower odds. These results align with the literature and larger studies that found a statistically significant relationship between BMI and stroke. Therefore, BMI category was included as a variable in the final predictive model.


# Final Feature Selection:
- Exclude Features: 
    - Gender Type
    - Marriage Status (Adjusted by age)
    - Residence Type

- Included Features: 
    - Smoking Status
    - Age
    - Glucose Level
    - Hypertension
    - Heart Disease
    - Work Type
    - BMI


