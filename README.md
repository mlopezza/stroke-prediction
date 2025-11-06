# stroke-prediction
Predicting the occurence of stroke in individuals using demographic and health-related factors


## GITHUB REPO:
https://github.com/Abayomiokojie/stroke-prediction.git

### Data Science Project Notebook: 

### Expectations for Week 1
After Week 1, you will be evaluated on your project's README file. By this point, it must
include a detailed project proposal. This should include the business motivation for your
project, the dataset you have chosen to use, and any risks or unknowns you have identified.


## Data Science Institute- Cohort 7- Team DS3 - Final Project

## Team Members DS 3: 
- Joshua Okojie	
- Lindsay Hudson
- Mariluz Lopez Zamora
- Yunpeng Wang


## RepoTitle: Stroke Prediction
### README Title options: 
- Stroke Risk Stratification: Identifying Vulnerable Individuals through Data Visualization. 
- Profiling Individuals at Risk of Stroke: A Data-Driven Approach
- Characterization of Stroke Risk Populations: Clinical and Demographic Insights
- Understanding Stroke Risk: Patterns, Predictors, and Profiles
- Mapping the Landscape of Stroke Risk: A Characterization Study


## CHOSEN DATASET: Stroke Prediction Dataset
- URL: https://www.kaggle.com/datasets/fedesoriano/stroke-prediction-dataset

## Project Overview
-  Purpose and Overview
-  Exploratory Data Analysis

## Purpose and Overview
This project has as goal to analyze and visualize the stroke prediction dataset to obtain a target model of individuals most at risk of suffering from a stroke to develop effective preventative measures/campaigns to reduce the prevalence of stroke.

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
- dataset contains some N/A values in bmi, and unknowns in smoking_status column (data unavailable for patient). How are we going to handle them

 - bmi missing values: NaN: 201 (4% of dataset - this is within the acceptable range for using imputation with minimal risk to dataset),  with min value 10.3 and max value 97.6, and mean of 28.8: Data imputation with KNN. We should think about scaling the numerical data before using KNN for imputation.

 -  smoking_status: Unknown: 1544 (30% of dataset - with missing data over 10% being associated with bias in the statistical analysis, more advanced methods of imputation being required for larger percentages of missing data, and the data being missing not at random (MNAR) due to the nature of the willingness of respondents to disclose their smoking status, we decided not to use imputation for such a large portion of our dataset). Only 3 options: never smoked, formerly smoked, and smokes. Unknown be used as a 4th category. (Missing data will be its own category).


- Residence column title is capitalized while other columns are not
- work_type strings need to be standardized as capitalized or not
- smoking_status strings need to be standardized as capitalized or not
- do float types (avg_glucose and bmi) need to be standardized in terms of the number of decimal places (one has two places, the other has one place)?
- do we need to scale numerical data (especially before using KNN for imputation?
- There are potential  biases?,if somebody finds one, write it here.
- Think about the ethical part in insurance 


## Data Exploration: 

### Identifying correlations: Preliminary visualization of data to understand patterns, correlations and data distribution.
- How many observations with stroke we have? (insert the first graphic)

### Consider demographic factors in dataset: 
- How many womans and mens with stroke
- Characterization of stroke: 
- How many married and unmarried
- How many living in urban setting and how many in city
- How old are the people with strokes?
- Where worked people with stroke? 
- How many has HTA
- How many has heart_disease
- Glucose average: 
	- min value is 55, max value is 271, we can’t take the mean, 
	- we could separated in groups: to take in consideracion, aleatory glucose test  in value more than 200 mg/dl is diagnosis of Diabetes, therefore it is other comorbidity (we need to consider that in the analysis, it is another risk factor, sometimes patients arrive ‘without chronic diseases’ but they have it underdiagnosticated several years ago
	-  I suggest this groups according with the American society of diabetes:  
		- 140 mg/dl or less (use to be a normal value)
		- 141 to 199 mg/dl  (prediabetes or oral glucose intolerance)
		- More than 200 mg/dl Diabetes.

- BMI: min value 10.3 and max value 97.6
	- We can use de BMI classification from the CDC: 
		- <18.5: Underweight
		- 18.5>25 Healthy Weight
		- 25>=30: Overweight
		- >= 30 Obesity
		- 30 to 35 Obesity class 1
		- 35 to 40 Obesity class 2
		- >=40 Obesity class 3 (severe Obesity)

- Stroke and smoke: 
	- “formerly smoked"
	- "never smoked"
	- "smokes" 
	- "Unknown












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
- Insurance Companies: Live insurance, to know insurable individuals ? to develop preventive strategies between the individuals that are actually insurable.
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


----



## References: 
- (1)https://www.canada.ca/en/public-health/services/publications/diseases-conditions/stroke-canada-fact-sheet.html
- Dong Y, Peng CY. Principled missing data methods for researchers. Springerplus. 2013 May 14;2(1):222. doi: 10.1186/2193-1801-2-222. PMID: 23853744; PMCID: PMC3701793.
- Junaid, K.P., Kiran, T., Gupta, M. et al. How much missing data is too much to impute for longitudinal health indicators? A preliminary guideline for the choice of the extent of missing proportion to impute with multiple imputation by chained equations. Popul Health Metrics 23, 2 (2025). https://doi.org/10.1186/s12963-025-00364-2
- https://diabetes.org/espanol/diagnostico
- https://www.cdc.gov/bmi/adult-calculator/bmi-categories.html
