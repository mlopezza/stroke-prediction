# Stroke Risk Stratification: Identifying Vulnerable Individuals through Data Visualization. 
Predicting the occurence of stroke in individuals using demographic and health-related factors.

### Type of project: Data Storytelling and Visualization Project.


## GitHub Repo:
https://github.com/Abayomiokojie/stroke-prediction.git

## Repository Structure:
	├── data
	├──── processed
	├──── raw
	├──── sql
	├── experiments
	├── models
	├── reports
	├── src
	├── README.md
	└── .gitignore

- Data: Contains the raw, processed and final data. For any data living in a database, make sure to export the tables out into the sql folder, so it can be used by anyone else.
- Experiments: A folder for experiments.
- Models: A folder containing trained models or model predictions.
- Reports: Generated HTML, PDF etc. of your report.
- src: Project source code.
- README: This file
- .gitignore: Files to exclude from this folder (e.g., large data files).

### Expectations for Week 1
After Week 1, you will be evaluated on your project's README file. By this point, it must include a 
- detailed project proposal. 
- business motivation for your project, 
- the dataset you have chosen to use, 
-  any risks or unknowns you have identified.


## Data Science Institute- Cohort 7- Team DS3 - Final Project

## Team Members DS 3: 
- Joshua Okojie	
- Lindsay Hudson
- Mariluz Lopez Zamora


### README Title other options: 
- Profiling Individuals at Risk of Stroke: A Data-Driven Approach
- Characterization of Stroke Risk Populations: Clinical and Demographic Insights
- Understanding Stroke Risk: Patterns, Predictors, and Profiles
- Mapping the Landscape of Stroke Risk: A Characterization Study


## Chosen Dataset: Stroke Prediction Dataset
- URL: https://www.kaggle.com/datasets/fedesoriano/stroke-prediction-dataset

## Project Overview
-  Purpose and Overview
- Methodology
-  Exploratory Data Analysis
- Findings and Visualization Analysis
- References

## Purpose and Overview
This project has as goal to analyze and visualize the stroke prediction dataset to obtain a target model of individuals most at risk of suffering from a stroke. The goal is to develop effective preventive measures and campaigns to reduce the prevalence of stroke.

### Business Problem
We are a group of data scientists concerned about the high mortality and disability associated with stroke. In Canada, it is the third leading cause of death and the tenth largest contributor to disability-adjusted life years (the number of years lost due to ill health, disability, or early death)(1). It impacts health system costs, the workforce, and society. 

Our job is to characterize individuals at the highest risk of stroke based on demographic and risk factors. This profile will enable the identification of the target population that could benefit from health prevention campaigns, as well as marketing campaigns with industries that develop products associated with health, fitness, and healthy food.

We worked with the Stroke Prediction Dataset ((it is possible, insert the link here), which is public through kaggle. It consists of 5.110 observations with 11 attributes between demographic  and possible risk factors like health comorbidities and lifestyle information, including the variable ‘stroke’ which is  1 if the patient had a stroke or 0 if not. 

After a preliminary analysis of the dataset we classify the attributes (columns)  in four big groups defined as: 
- Immutable demographic characteristics: Demographic characteristics that are not changeable like gender and age.
- Mutable demographic characteristics: Demographic characteristics that are potentially changeable like marital status, work and residence type.
- Immutable Risk: aspects related to the individual that could be related with major risk of stroke and  are not alterable like  chronic comorbidities: heart disease and  hypertension.
- Mutable Risk Factors: aspects related to the individual lifestyle that could be related with major risk of stroke and  are alterable like  average glucose level, body mass index and smoking status.

#### Name of Columns:
1) id: unique identifier
2) gender: "Male", "Female" or "Other"
3) age: age of the patient
4) hypertension: 0 if the patient doesn't have hypertension, 1 if the patient has hypertension
5) heart_disease: 0 if the patient doesn't have any heart diseases, 1 if the patient has a heart disease
6) ever_married: "No" or "Yes"
7) work_type: "children", "Govt_jov", "Never_worked", "Private" or "Self-employed"
8) Residence_type: "Rural" or "Urban"
9) avg_glucose_level: average glucose level in blood
10) bmi: body mass index
11) smoking_status: "formerly smoked", "never smoked", "smokes" or "Unknown"*
12) stroke: 1 if the patient had a stroke or 0 if not
*Note: "Unknown" in smoking_status means that the information is unavailable for this patient





## METHODOLOGY

### DATASET CLEANING:
Exploration of  Types of data in the dataset to standardize it. Handle missing values, removing inconsistencies and ensuring data readiness. 

| Feature         | Type                                                                 |
|-----------------|----------------------------------------------------------------------|
| gender          | String, binary (male/female)                                         |
| age             | Integer                                                              |
| hypertension    | Integer, binary (0/1)                                                |
| heart_disease   | Integer, binary (0/1)                                                |
| ever_married    | String, binary (yes/no)                                              |
| work_type       | String, non-binary (children, Govt_job, Never_worked, Private, Self-employed) |
| Residence       | String, binary (urban/rural)                                         |
| avg_glucose     | Float (2 decimal places)                                             |
| bmi             | Float (1 decimal place)                                              |
| smoking_status  | String, non-binary (formerly smoked, never smoked, smokes, Unknown)  |
| stroke          | Integer, binary (0/1)                                                |




#### IDENTIFIED RISK AND UNKNOWNS AND RESOLVE MISSING VALUES:

##### Missing Values:
- It was found some 'N/A' values in 'bmi' column, and 'unknowns' in 'smoking_status' column (data unavailable for patient). 
- After careful consideration and analysis, and taking into account the different types of data each column provides, we handle them as follows:
	- BMI missing values (NaN) represent 201 observations, which corresponds to 4% of the dataset. This is within the acceptable range for using imputation with minimal risk to the dataset. Furthermore, the data is considered Missing Not at Random (MNAR) because it depends on the respondents’ willingness to disclose their weight. 

		However, the BMI column has a minimum value of 10.3 and a maximum value of 97.6, with a mean of 28.8. Because of the large gap between these values, we decided to handle the missing data in this column through imputation using KNN with a number of neighbords = 9.


	- The smoking_status value “Unknown” represents 1,544 observations, which corresponds to 30% of the dataset. In addition, this attribute only allows three answer options: never smoked, formerly smoked, and smokes.
	
		Since missing data greater than 10% can introduce bias into statistical analysis, and more advanced imputation methods are usually required for larger percentages of missing data, we decided not to use imputation in this case. Furthermore, the data is considered Missing Not at Random (MNAR) because it depends on the respondents’ willingness to disclose their smoking status.

		Therefore, missing data will be treated as its own category, with “Unknown” used as a fourth category.


##### Standardization:
- Residence column title is capitalized while other columns are not.
- work_type strings need to be standardized as capitalized or not
- smoking_status strings need to be standardized as capitalized or not
- do float types (avg_glucose, age and bmi) need to be standardized in terms of the number of decimal places (one has two places, the other has one place).
- do we need to scale numerical data (especially before using KNN for imputation?


##### Ethical considerations:
- 



### Data Exploration: 

#### Identifying correlations: Preliminary visualization of data to understand patterns, correlations and data distribution.
- How many observations with stroke we have? (insert the first graphic)

- After extract the observations with stroke we figuraud a big imbalances in the dataset, we only found: 249 obervations with stroke. It means only 4.9% of the observations.  
- In adition, the big difrence between the BMI values, with  min value 10.3 and max value 97.6, made difficult the visualization of the correlation Stroke vs. BMI, for this reason we used the BMI classification from the CDC:
	- <18.5: Underweight
	- 18.5>25 Healthy Weight
	- 25>=30: Overweight
	- => 30 Obesity
- The big diference between average glucose level with min value in 55, and max value in 271, was an issue as well, we decide to use the American Diabetes Asociation  clasification:  
	- les tan 70 mg/dl: Low glucosa level
	- 70 yo 140 mg/dl Healthy Value
	- 141 to 199 mg/dl: prediabetes or oral glucose intolerance.
	- More than 200 mg/dl: Diabetes.
- We used the canadian clasification for age: 
	- children 0-14 years old
	- youth 15-25 years old
	- Adult 25 to 64 years old
	- Senior 65 and more years old

## Findings and Analysis

### Demographic Characterization of observations with and without stroke: 

- Immutable demographic characteristics: 
	- Gender: 
		- From the total observations, 41.4% of them was men and 58.6% was women.
		- From our 249 observations of stroke, 43.4% of them was men and 56.6% was women.

	- Age: we could observed a increase of stroke incidence after 57 years old with a peak after 78 years  old.
		- 31.7% of observations had 57 or more years old and 68.3% had less than 57 years old. 
		- --- % was children between 0-14 years old
		- --- % was youth between 15-25 years old
		- ----%was Adult between 25 to 64 years old
		- ---- % was Senior between 65 and more years old

- Mutable demographic characteristics: 
	- Marital status: 
		- 
		- Stroke: 88.4% was married and 11.6%% was never merried, however, --% of stroke observation had more than--years old, therefore, it is an inbalance of categorie and is dificult to make a conclution here. 

	- Setting: --% living in urban setting and --%  in city	
	- Type of work: Where worked people with stroke? 

- Immutable Risk: 
	- HTA: ---% of patients with stroke has HTA,  and % of patients without a stroke  have it. 
	- Heart_disease --- % of patients with stroke has HTA and % of patientes without stroke have it. 
	We found that patients with stroke use to have more % of HTA and HD?


- Mutable Risk Factors: 
	- Glucose average: Acording with the American society of diabetes clasification, and in base of glucose average, %-- of the patients had an average in the diagnosis range of diabetes that was not reported in the dataset information or used as charactertistic, also, % had an average of glucose in   oral glucose intolerance and only %--- have a normal value. 
		- when we compare with patients without stroke, only---% of then had glucose average in diabetic range, % in oral glucose intolerance and %.--- had a normal value. 

	- BMI: Using the BMI clasification from CDC, from patients with stroke,  ---% had Obesity, --% had  Overweight, --% had Healthy Weight and %.. had Underweight, the % of patients without stroke %--- 
		
	- Stroke and smoke: when we compare the porcentage of patient with stroke who are "smokes and formerly smoked", it is a biger percent of patients without stroke?
	- to keep in mind: 40 observation with stroke and smoking status unknown. 
	- “formerly smoked"
	- "never smoked"
	- "smokes" 
	- "Unknown









## Final outcome of the project: model

## 




## TECHNICAL STACK: 
### Programming Language:
- Python
- SQL???

### Libraries Used
- Numpy: matrix operations
- Pandas: data analysis
- Matplotlib: creating graphs and plots
- Plotly: creating graphs and plots
- Seaborn: enhancing matplotlib plots
- SKLearn: classification analysis

## PROJECT SCOPE
	Description:



### Stakeholders: 
- Health providers: to develop preventive strategies that could save money to the health system.
- Insurance Companies: Live insurance, to know insurable individuals ? to develop preventive strategies between the individuals that actually have a live insurance.
- Marketing segmentation: 
	- industries that develop products that help to prevent it
	- industries that develop products that could help with stroke  disability?
	- Industries that promote wellbeing and fitness
	- Industries related to healthy food.



## Understanding the Data:

- What value does your project bring to the industry?
- How will you answer your business question with your chosen dataset?
- What are the risks and uncertainties?
- What methods and technologies will you use?

## Next Task: 
- Clean and standardize the data
- Preliminary visualization of the correlation between the variables
- Made graphics stroke vrs without stroke: HTA, HD, Smoke, Glucose, BMI

## 

----

## Team member's videos
- Joshua Okojie	
- Lindsay Hudson
- Mariluz Lopez Zamora


## References: 
- (1)https://www.canada.ca/en/public-health/services/publications/diseases-conditions/stroke-canada-fact-sheet.html
- Dong Y, Peng CY. Principled missing data methods for researchers. Springerplus. 2013 May 14;2(1):222. doi: 10.1186/2193-1801-2-222. PMID: 23853744; PMCID: PMC3701793.
- Junaid, K.P., Kiran, T., Gupta, M. et al. How much missing data is too much to impute for longitudinal health indicators? A preliminary guideline for the choice of the extent of missing proportion to impute with multiple imputation by chained equations. Popul Health Metrics 23, 2 (2025). https://doi.org/10.1186/s12963-025-00364-2
- https://diabetes.org/about-diabetes/diagnosis
- https://www.cdc.gov/bmi/adult-calculator/bmi-categories.html
<<<<<<< Updated upstream
=======
- https://www.statcan.gc.ca/en/concepts/definitions/age2

>>>>>>> Stashed changes
