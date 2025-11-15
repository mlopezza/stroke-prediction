# Stroke Risk Stratification: Identifying vulnerable individuals through data visualization

Predicting the occurence of stroke in individuals using demographic and health-related factors.

## Type of project

- Data storytelling and visualization with a predictive model.
- Data Science Institute- Cohort 7- Team DS3 - Final Project

## Repository Structure

    ├── data
    ├──── processed
    ├──── raw
    ├── experiments
    ├── images
    ├── models
    ├── reports
    ├── src
    ├── README.md
    └── .gitignore

- Data: raw, processed and final data.
- Experiments: Experiments.
- Images: Final images.
- Models: Trained models or model predictions.
- Reports: Generated HTML, PDF etc. of the analysis report.
- src: Project source code.
- README: This file.
- .gitignore: Files to exclude from this folder.

## Team Members

- Joshua Okojie
- Lindsay Hudson
- Mariluz Lopez Zamora

## Chosen Dataset: Stroke Prediction Dataset

- URL: <https://www.kaggle.com/datasets/fedesoriano/stroke-prediction-dataset>

## Project Overview

- [Purpose and Overview](#purpose-and-overview)
- [Methodology](#methodology)
- [Data Analysis](#data-exploration)
- [Predictive Model](#predictive-model)
- [Technical Stack](#technical-stack)
- [Team Member´s Videos](#team-members-videos)
- [References](#references)

## Purpose and Overview

This project has as goal to analyze and visualize the stroke prediction dataset to obtain a target model of individuals most at risk of suffering from a stroke. The goal is to develop effective preventive measures and campaigns to reduce the prevalence of stroke.

### Business Problem

We are a group of data scientists concerned about the high mortality and disability associated with stroke. In Canada, it is the third leading cause of death and the tenth largest contributor to disability-adjusted life years (the number of years lost due to ill health, disability, or early death)(1). It impacts health system costs, the workforce, and society.

Our job is to characterize individuals at the highest risk of stroke based on demographic and risk factors. This profile will enable the identification of the target population that could benefit from health prevention campaigns, as well as marketing campaigns with industries that develop products associated with health, fitness, and healthy food.

We worked with the Stroke Prediction [Dataset](https://www.kaggle.com/datasets/fedesoriano/stroke-prediction-dataset), which is public through kaggle. It consists of 5.110 observations with 11 attributes between demographic and possible risk factors like health comorbidities and lifestyle information, including the variable ‘stroke’ which is 1 if the patient had a stroke or 0 if not.

After a preliminary analysis of the dataset we classify the attributes (columns) in four big groups defined as:

- Immutable demographic characteristics: Demographic characteristics that are not changeable like gender and age.
- Mutable demographic characteristics: Demographic characteristics that are potentially changeable like marital status, work and residence type.
- Immutable Risk: aspects related to the individual that could be related with major risk of stroke and are not alterable like chronic comorbidities: heart disease and hypertension.
- Mutable Risk Factors: aspects related to the individual lifestyle that could be related with major risk of stroke and are alterable like average glucose level, body mass index and smoking status.

#### Features description:

| Feature        | Type                     | Description                                                                                   |
| -------------- | ------------------------ | --------------------------------------------------------------------------------------------- |
| id             | Integer, unique          | Unique identifier for each patient                                                            |
| gender         | String, binary           | "Male", "Female", or "Other"                                                                  |
| age            | Integer                  | Age of the patient                                                                            |
| hypertension   | Integer, binary (0/1)    | 0 = patient does not have hypertension; 1 = patient has hypertension                          |
| heart_disease  | Integer, binary (0/1)    | 0 = patient does not have heart disease; 1 = patient has heart disease                        |
| ever_married   | String, binary           | "Yes" or "No"                                                                                 |
| work_type      | String, non-binary       | Patient’s type of work: "children", "Govt_job", "Never_worked", "Private", or "Self-employed" |
| residence      | String, binary           | "Urban" or "Rural"                                                                            |
| avg_glucose    | Float (2 decimal places) | Average glucose level in blood                                                                |
| bmi            | Float (1 decimal place)  | Body Mass Index                                                                               |
| smoking_status | String, non-binary       | "Formerly smoked", "Never smoked", "Smokes", or "Unknown"\*                                   |
| stroke         | Integer, binary (0/1)    | 1 = patient had a stroke; 0 = patient did not have a stroke                                   |

_Note: "Unknown" in smoking_status means that the information is unavailable for this patient._

## Methodology

### Data Exploration

#### Identified Risk and Unknown and Resolve Missing Values

##### Missing Values

- It was found some 'N/A' values in 'bmi' column, and 'unknowns' in 'smoking_status' column (data unavailable for patient).
- After careful consideration and analysis, and taking into account the different types of data each column provides, we handle them as follows:

  - BMI missing values (NaN) represent 201 observations, which corresponds to 4% of the dataset. This is within the acceptable range for using imputation with minimal risk to the dataset. Furthermore, the data is considered Missing Not at Random (MNAR) because it depends on the respondents’ willingness to disclose their weight.

    However, the BMI column has a minimum value of 10.3 and a maximum value of 97.6, with a mean of 28.8. Because of the large gap between these values, we decided to handle the missing data in this column through imputation using KNN with a number of neighbords = 9.

    ![Data Imputation with KNN](images/stroke_comparative_visualizations/data_imputation_KNN.png)

  - The smoking_status value “Unknown” represents 1,544 observations, which corresponds to 30% of the dataset. In addition, this attribute only allows three answer options: never smoked, formerly smoked, and smokes.

    Since missing data greater than 10% can introduce bias into statistical analysis, and more advanced imputation methods are usually required for larger percentages of missing data, we decided not to use imputation in this case. Furthermore, the data is considered Missing Not at Random (MNAR) because it depends on the respondents’ willingness to disclose their smoking status.

    Therefore, missing data will be treated as its own category, with “Unknown” used as a fourth category.

##### Standardization

- The Residence column title was capitalized while other column titles were not; this was corrected.
- Some feature values such as work_type, smoking_status, ever_married, and residence were kept with the first letter capitalized, as in the original dataset.
- The age column was left with its original number of decimal places because it includes observations of less than one year, which could be altered otherwise.
- The float types (avg_glucose and bmi) were standardized in terms of the number of decimal places.

#### Identifying correlations: Preliminary visualization of data to understand patterns, correlations and data distribution (insert link to Graphs Lindsay and Mary)

![Observations with stroke](images/stroke_comparative_visualizations/comparative_total.png)

- After extract the observations with stroke we figuraud a big imbalances in the dataset, we only found: 249 obervations with stroke. It means only 4.9% of the observations and two of them was pediatric population.

- Adjustment for Age and Physiological Differences: After extensive analysis, it was decided to reduce confounding factors associated with age and physiological differences between adults (≥18 years old) and the pediatric population (<18 years old). For example, different tools and reference values are used to determine healthy weight and glucose levels in pediatric populations. Additionally, the incidence of pediatric stroke is estimated at approximately 1.2 to 13 cases per 100,000 children under 18 years old (2). In contrast, adults experience a much higher incidence, with nearly 151 cases per 100,000 individuals per year (3).

- The big difrence between the BMI values, with min value 10.3 and max value 97.6, made difficult the visualization of the correlation Stroke vs. BMI. Further, pedaitric pupulation had a different interpretation of BMI that is based on percentiles and standar deviations, this afect the interpretation of this variable because pediatric observations could be missclasificated as low weight. After exclude the pediatric population we used the BMI classification from the CDC (4):

  - <18.5: Underweight
  - 18.5>25 Healthy Weight
  - 25>=30: Overweight
  - => 30 Obesity
    ![Stroke by BMI](images/Stroke%20Positive%20Visualizations/stroke_patients_by_bmi.png)

- The big diference between average glucose level with min value in 55, and max value in 271, was an issue as well, after remove the pediatric population, we decide to use the American Diabetes Asociation clasification (5):

  - les tan 70 mg/dl: Low glucosa level
  - 70 yo 140 mg/dl Healthy Value
  - 141 to 199 mg/dl: prediabetes or oral glucose intolerance.
  - More than 200 mg/dl: Diabetes.

  ![stroke by gluc level](images/Stroke%20Positive%20Visualizations/stroke_patients_by_glucose.png)

- We used the canadian clasification for age (6), excluding children categorie and asuming the impact on the youth categorie after exclude observations with less than 18 years old:
  - children 0-14 years old
  - youth 15-25 years old
  - Adult 25 to 64 years old
  - Senior 65 and more years old

## Data Analysis

### Demographic Characterization of observations with and without stroke

- Immutable demographic characteristics:

  - Gender:
    - From the total observations, 41.4% of them was men and 58.6% was women.
    - From our 249 observations of stroke, 43.4% of them was men and 56.6% was women.

  ![stroke comparaative by gender](images/stroke_comparative_visualizations/comparative_by_gender.png)

  - Age: we could observed a increase of stroke incidence after 57 years old with a peak after 78 years old.

  ![age peak](images/Stroke%20Positive%20Visualizations/stroke_patients_by_age.png)

  - 31.7% of observations had 57 or more years old and 68.3% had less than 57 years old.
  - --- % was children between 0-14 years old
  - --- % was youth between 15-25 years old
  - ----%was Adult between 25 to 64 years old
  - ---- % was Senior between 65 and more years old

![age means](images/stroke_comparative_visualizations/comparative_by_age_category.png)

- Mutable demographic characteristics:
  ![mutab demog feat](<images/Stroke%20Positive%20Visualizations/stroke_patients_categorical(2).png>)

  - Marital status:
  - - Stroke: 88.4% was married and 11.6%% was never merried, however, --% of stroke observation had more than--years old, therefore, it is an inbalance of categorie and is dificult to make a conclution here.

    ![stroke by married](<images/Stroke%20Positive%20Visualizations/stroke_patients_categorical(1).png>)

    - Setting: --% living in urban setting and --% in city
    - Type of work: Where worked people with stroke?

- Immutable Risk:
  ![stroke comorb](images/Stroke%20Positive%20Visualizations/stroke_patients_comorbidity.png)

  - HTA: ---% of patients with stroke has HTA, and % of patients without a stroke have it.
  - Heart_disease --- % of patients with stroke has HTA and % of patientes without stroke have it.
    We found that patients with stroke use to have more % of HTA and HD?

![stroke compar comorb](images/stroke_comparative_visualizations/comparative_by_comorb.png)

- Mutable Risk Factors:

  - Glucose average: Acording with the American society of diabetes clasification, and in base of glucose average, %-- of the patients had an average in the diagnosis range of diabetes that was not reported in the dataset information or used as charactertistic, also, % had an average of glucose in oral glucose intolerance and only %--- have a normal value.

  ![stroke with gluc](images/Stroke%20Positive%20Visualizations/stroke_patients_by_glucose.png)

  - when we compare with patients without stroke, only---% of then had glucose average in diabetic range, % in oral glucose intolerance and %.--- had a normal value.

![stroke comp gluc](images/stroke_comparative_visualizations/comparative_by_gluc_category.png)

- BMI: Using the BMI clasification from CDC, from patients with stroke, ---% had Obesity, --% had Overweight, --% had Healthy Weight and %.. had Underweight, the % of patients without stroke %---

![stroke comp bmi](images/stroke_comparative_visualizations/comparative_by_bmi.png)

- Stroke and smoke: when we compare the porcentage of patient with stroke who are "smokes and formerly smoked", it is a biger percent of patients without stroke?

![stroke comp smoke](images/stroke_comparative_visualizations/comparative_by_smok_stat.png)

### Statistical Analysis (insert link to data_analyst file)

Selecting relevant features and observations to include in the model, while removing variables that could act as confounding factors to ensure model accuracy.

#### Statistical Analyst of Features

A test of independence was conducted to determine which features were significant among stroke patients. The T-test was applied to continuous numerical variables and the Chi Square was applied on categorical variables . These results helped guide decisions on which features to include in building our predictive stroke model.

The features of age, smoking status, glucose level, hypertension, heart disease, work type, and BMI were all significant (p-value , <0.05) and were included in training the model. The features of gender, residence type, and marriage status were excluded from training the model. Gender (p-value > 0.05) and residence type (p-value > 0.05) were not significant and thus excluded from the model based on their lack of impact. Marriage status was significant (p-value < 0.05) however, due to the significance of age among stroke patients, and the influence of age on marriage status (ie. older individuals are more likely to be married). There were concerns that including marital status could lead to an overestimation of its effect, since age acts as a confounding factor for this variable. After a statistical analyst , adjusting married status by age the difference was not significant (p-value > 0.05), and the first result was explained due to the significance of age among stroke patients, the decision was made to exclude it from the model.

##### Final selection of features for model

![stroke factors](images/Stroke%20Positive%20Visualizations/stroke_patients_bubble.png)

- Exclude Features: Not significant (p-Value >0.5)

  - Gender Type
  - Marriage Status (Adjusted by age)
  - Residence Type

- Included Features: Significant (p-Value <0.5)
  - Smoking Status
  - Age
  - Glucose Level
  - Hypertension
  - Heart Disease
  - Work Type
  - BMI

## Predictive Model

- Classification Analysis and Validation: Applying linear classification models to identify individuals aged 18 or older who are most at risk of developing stroke, based on the selected features. Additionally, creating training and test sets and assessing model accuracy.

- Visualization: Creating plots to represent insights and model results.

- Conclusion

## Project Scope

This project focuses on conducting a classification analysis using demographic and health-related data from individuals aged 18 to 82 years. The goal is to identify key factors that predict which individuals are most at risk of developing stroke.

To ensure the reliability of the analysis, we will apply data splitting techniques and evaluate performance metrics.

In addition, the project will provide insights and recommendations to highlight which groups of individuals may benefit most from prevention campaigns and healthy lifestyle strategies based on the findings.

![model](images/Model%20Visualizations/model_3_visualization.png)

### Stakeholders

- Health Providers

  - Develop preventive strategies that can reduce costs for the healthcare system.

  - Use insights to improve patient care and early intervention.

- Insurance Companies

  - Life insurance providers interested in identifying insurable individuals.

  - Develop preventive strategies for policyholders to reduce long‑term risk and claims.

- Marketing Segmentation

  - Preventive product industries: companies that design products to reduce stroke risk.

  - Rehabilitation and disability support industries: organizations that provide products or services to help patients living with stroke‑related disabilities.

  - Wellbeing and fitness industries: businesses promoting exercise, lifestyle improvement, and overall health.

  - Healthy food industries: companies focused on nutrition and dietary products that support stroke prevention.

## Technical stack

### Programming Language

- Python

### Libraries Used

- Numpy: matrix operations
- Pandas: data analysis
- Matplotlib: creating graphs and plots
- Plotly: creating graphs and plots
- Seaborn: enhancing matplotlib plots
- SKLearn: classification analysis

---

## Team member's videos

- Joshua Okojie
- Lindsay Hudson
- Mariluz Lopez Zamora

## References

- (1)<https://www.canada.ca/en/public-health/services/publications/diseases-conditions/stroke-canada-fact-sheet.html>
- (2) <https://pmc.ncbi.nlm.nih.gov/articles/PMC9856134/>
- (3) <https://pmc.ncbi.nlm.nih.gov/articles/PMC11786524/>
- (4) <https://www.cdc.gov/bmi/adult-calculator/bmi-categories.html>
- (5) <https://diabetes.org/about-diabetes/diagnosis>
- (6) <https://www.statcan.gc.ca/en/concepts/definitions/age2>
- Dong Y, Peng CY. Principled missing data methods for researchers. Springerplus. 2013 May 14;2(1):222. doi: 10.1186/2193-1801-2-222. PMID: 23853744; PMCID: PMC3701793.
- Junaid, K.P., Kiran, T., Gupta, M. et al. How much missing data is too much to impute for longitudinal health indicators? A preliminary guideline for the choice of the extent of missing proportion to impute with multiple imputation by chained equations. Popul Health Metrics 23, 2 (2025). <https://doi.org/10.1186/s12963-025-00364-2>
  <https://python-graph-gallery.com/>
