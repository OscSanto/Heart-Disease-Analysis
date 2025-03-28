# Heart-Disease-Analysis using SVM in R
Using various data science techniques, this project hopes to find Heart Disease factors which can assist in discovering and identifying high risk individuals. 

We primarily focus on building a classifier which can predict heart disease based on patient clinical data, using Support Vector Machine (SVM) algorithm in R.

For this classification problem SVM was chosen for:
- Handling Non-linear Boundaries: Many relionships may not be linearly sepearable. SVM kernels can give us flexibilty to model these complex boundaries effectively.
- Dataset sizes: SVMs perform well with small to medium datasets.
- Robust to outliers: SVm focuses on the margin, giving it robustness against noisy observations which is common in clinical data.
- Effective in High Dimensions: SVMs in high dimensional space are less prone to overfit, especially when feature scaling and kernel tricks are included.

## 📌 Dataset Features
- **Age** - Patient's age in years
- **Sex** - (1 = male, 0 = female)
- **Chest pain type** -
        -- Value 1: typical angina
        -- Value 2: atypical angina
        -- Value 3: non-anginal pain
        -- Value 4: asymptomatic
- **BP (Resting Blood Pressure)** - Resting blood pressure in mm Hg
- **Cholesterol** - serum cholestoral in mg/dl
- **FBS over 120 (Fasting Blood Sugar)** - fasting blood suger > 120 mg/dl (1 = true; 0 = false)
- **EKG results** - Resting electrocardiographic results (normal, ST-T wave abnormality, left ventricular hypertropy)
- **Max HR (Maximum Heart Rate)** - Maximum heart rate during physical activity
- **Exercise Angina** - Agina occurance during exercise (1 = yes, 0, = no)
- **ST depression** - ST depression induced by exercise relative to rest 
- **Slope of ST** - Slope of the peak exercise ST segment (upsloping, flat, downsloping)
- **Number of vessels fluro** - Number of major vessels colored by fluoroscopy (0-3)
- **Thallium** - Thallium stress test (normal, fixed, defect, reversible defect)
- **Heart Disease** (target) - Heart disease present (1 = disease, 0 = absense)

## Approach
Preprocessing and feature evaluation centered around the nature of the dataset
### Data preprocessing
- Categorical interpeatoin: some variables (Chest pain type, Slope of ST, Thallium) are ordinal, containing a meaningful order that does not relate to a numerical distance between them. For these features, Spearman's rank correlation is more suited for the task than Pearson's (explain why pearosn is no good)
- Binary Features: Sex, FBS over 120, Exercise Angina are binary categorical variables that are handled using Pearson correlation since they are numerically interpreted as 0 and 1.
- Target conversion: The target variable (Heart.Disease) column was converted into a binary factor, making presense (1) and absense (0)
- Feature grouping
  Continous Numeric: Age, BP, Cholesterol, Max HR, ST depression
  Binary: Sex, FBS over 120, Exercise Angina, Heart Disease
  Ordinal: Chest pain type (1:4), EKG results (0,1,2), Slope of ST (1,2,3). Number of vessels fluro (0,1,2,3), Thallium (3,6,7)
- Feature Scaling: All continuous features are standardized using scale(), applying z-score normalization.
### Correlation Analysis
- To understand how each feature relates to heart disease: Pearson correlation for continuous and binary features. Spearman correlation for ordinal variables
- Both linear and rank-based relationships are captured
<pre>
                                      Attribute Correlation    R_squared
Thallium                               Thallium  0.52527019 0.2759087675
Number.of.vessels.fluro Number.of.vessels.fluro  0.48157290 0.2319124558
Chest.pain.type                 Chest.pain.type  0.47006578 0.2209618397
Exercise.angina                 Exercise.angina  0.41930271 0.1758147619
Max.HR                                   Max.HR -0.41851397 0.1751539392
ST.depression                     ST.depression  0.41796744 0.1746967786
Slope.of.ST                         Slope.of.ST  0.36318164 0.1319009009
Sex                                         Sex  0.29772076 0.0886376484
Age                                         Age  0.22291432 0.0496907938
EKG.results                         EKG.results  0.18207142 0.0331500019
BP                                           BP  0.15538266 0.0241437698
Cholesterol                         Cholesterol  0.11802053 0.0139288456
FBS.over.120                       FBS.over.120 -0.01631883 0.0002663043
</pre>
        
### Interpreting
- Features like Thallium, Number of vessels fluro, chest pain type, show strong correlation with heart disease and are critical to classifcation. Notably, Max HR has negative correlation, suggesting people with lower maximum heart rate may have higher heart disease risk.
- FBS Cholesterol and BP are less predictive for this dataset
### Correlation Matrix Heatmap
This heatmap visualizes pairwise correlation across predictors to find potential multicollinearity that may negativeiy affect proceeding model predictions.
## Conclusion
After training and evaluating 3 SVM classifiers (linear, polynomial, radial) we gained insights into which can accurately predict heart disease from the dataset.
- Linear (cost = 0.01): performing the best, with lowest cross validation error and well generalization performance
- Polynomial (cost = 1, degree = 3): overfit the training data. Very low training error (6.5%) but no gain in test error.
- Radial (cost = 0.01, gamma = 2): underperformed, likely due to insufficent tuning or complexity of data

