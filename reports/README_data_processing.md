# Data Processing
## Missing Data
### Nan Values: Imputation with KNN

To handle the missing BMI values, we applied KNN imputation.

To use KNN, we selected only the features that could influence body weight. We focused on numerical columns, but since gender is a categorical variable with a clea r impact on BMI, we converted it into numeric values: Male = 1, Female = 0, Other = 2.

The numerical columns used for KNN imputation were: 'gender', 'age', 'hypertension', 'heart_disease', 'avg_glucose_level', 'bmi', 'stroke'.

The categorical columns excluded from the imputation because they have little or no impact on BMI were: 'ever_married', 'work_type', 'residence_type', 'smoking_status'.

We selected n_neighbors = 9 with weights = "uniform".

![KNN imputation](../images/stroke_comparative_visualizations/data_imputation_KNN.png)

### Unknown Values
Unknown data in smoking_status accounted for 30% of the dataset and was considered Missing Not at Random (MNAR) because it depended on the respondents’ willingness to disclose their smoking habits. Therefore, missing data were treated as a separate category, with “Unknown” used as a fourth category.

## Standardization
The column title Residence was originally capitalized while others were not; this was standardized.

Some feature values such as work_type, smoking_status, ever_married, and residence were kept with an initial capital letter to remain consistent with the original dataset.

The age column was kept with its original decimal values to preserve observations of children younger than one year.

The float features (avg_glucose_level and bmi) were standardized in terms of the number of decimal places.


## Notes on Age Values Below 1 Year

A deeper analysis of the age variable was required to understand the observations with values below one year.

When exploring the age distribution, we identified several observations with decimal ages. After checking related features—hypertension, heart_disease, ever_married, work_type, and smoking_status—we concluded that these observations are valid and correspond to real pediatric data.

For this reason, we decided not to round or remove the decimal values for this attribute.

Once the dataset was cleaned and standardized, the analysis was conducted.

## Anotations in relation with statistical analysis

### Confounding factors
A later removal of observations under 18 years old was performed in the subsequent statistical analysis step to further reduce confunding factors and increase accuracy on the final model.

As our goal was to support stroke prevention, we needed to consider the important differences between adults and the pediatric population. Because the causes of stroke in children are mostly related to genetic factors and other non-preventable conditions—and because the incidence is very low—the pediatric population was removed from the analysis and the model. This helped reduce confounding factors associated with their differences compared to adults and increased the accuracy of the model.

### Variability
Variability was handled in the statistical analysis through the application of standardized categories, including CDC categories for BMI, the Canadian classification for age, and the ADA classification for diabetes diagnosis.

