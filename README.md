# Predicting Food Insecurity
## Isabella Sun

## Introduction
Food insecurity is the disruption for food intake or patterns due to a lack of resources. Commonly, lack of resources means not having enough money to buy food. It can also mean not having physical access to food, by living in neighborhoods without full-service supermarkets and grocery stores and not having adequate access to transportation. This is important because individuals living in households that suffer from food insecurity (whether it be temporary or long-term), are at higher risk for negative health outcomes.<sup>[1](#foot1)</sup>

In this project, I build a model to predict household food insecurity using data from the Fed's 2020 Survey of Household Economics and Decisionmaking. 


## Data
I used data from the Federal Reserve Board's 2020 [Survey of Household Economics and Decisionmaking](https://www.federalreserve.gov/consumerscommunities/shed.htm) (SHED). This is an annual survey that asks respondents questions in a range of topics related to their financial well-being. 

There were 4165 households in the sample. 

### Feature Engineering Methodology
With the dataset containing many features to choose from, I narrowed down the relevant features to include in the model with 2 strategies. The first was by examining what other researchers and experts in the space have previously found to be relevant factors to food insecurity, and the second was to include features that would not be unique to this dataset/survey. 

1. Previous Research

    Previous reserach has found that the key factors that are closely related to food insecurity include poverty, unemployment, and home ownership.<sup>[2](#foot2)</sup> As such, these features are included in the model.

2. Selecting 'Useful' Features

    The data used to train this model comes from the 2020 SHED survey which includes specific questions related to COVID-19. For example, the survey asks respondents how they feel about precautions that their employer is taking to prevent the spread of COVID-19. While it is possible that a respondent's answer to that question could be a predictor of whether or not they have applied for SNAP benefits, this question is specific to this survey. Factors included in the model such as education, employment status, household size, race etc., are more easily obtained and would be more useful for predicting food insecurity with future datasets. 

### Key Variables

- **Target**: Applied for or received Supplemental Nutrition Assistance Program (SNAP)

    The question in the survey asks respondents: 
    "Since March 2020, have you and/or your spouse/partner either received or applied for each of the following forms of income or assistnace, or not? SNAP (sometimes known as Food Stamps)" 
    
    The possible responses were:
    - Refused
    - Received
    - Applied for but not received
    - Did not apply for and did not receive

    I defined a household as being food insecure if they applied for SNAP benefits or they have received SNAP benefits with the assumption that if a household has applied for this assistance, then they are in need of some form of income assistance to access food. 

- **Features**
    - Age
    - Gender
    - Race
    - Household size
    - Income
    - Education
    - Number of household members that were children 
    - Employment status
    - Retirement status
    - Whether they borrowed from retirement savings
    - Housing type
    - Ownership status of living quarters
    - Marital status
    - Living in a metro area
    - State/Region

## Exploratory Data Analysis

The proportion of the sample that are labeled as food insecure is **0.1102**. 

Poverty is an important factor related to food insecurity. The below figure compares the proportion of food insecure households whose annual income falls within the various income categories and proportion of not food insecure households whose annual income falls within the various income categories. There is a greater proportion of food insecure households at the lower  


![alt text](images/prop_incomecat.png "Title")


## Model Selection/Tuning

- Train test split

    I reserved 20% of the data as a test set to measure the performance of the models. 
    
    With only a little over 10% of the observations having applied for SNAP benefits, to handle the class imbalance, I stratified the split so that the proportion of the minority class is preserved between the train and test sets. 

- F1 score

    I chose to use the F-score to measure the performance of each model because it is a balance between precision and recall and because of the previously mentioned class imbalance in the data. 

![alt text](images/gb_bosstingstages.png "Title")

| Model         | Precision     | Recall       | F1-Score     | 
| ------------- | ------------- |------------- |------------- |
| Decision Tree | 0.3723  | 0.3804 | 0.3763 | 
| Random Forest (no tuning) | 0.6944  | 0.2717 | 0.3906 | 
| Random Forest (grid search CV) | 0.4265  | 0.6304 | 0.5088 | 
| Gradient Boost Classifier (no tuning)| 0.6226  | 0.3587 | 0.4552 | 
| Gradient Boost Classifier (grid search CV)| 0.775  | 0.3370 | 0.4697 | 


**35% improvement from the base model (decision tree) to the grid search random forest model**


## Important Features
The figure below shows the top 5 most important features for predicting food insecurity. 
![alt text](images/rf_feature_importance.png "Title")


_____________________________________________________________________

<a name="foot1">[1]</a> [Office of Disease Prevention and Health Promotion](https://www.healthypeople.gov/2020/topics-objectives/topic/social-determinants-health/interventions-resources/food-insecurity#5)

<a name="foot2">[2]</a> [Feeding America: Map the Meal Gap](https://www.feedingamerica.org/sites/default/files/2020-09/Map%20the%20Meal%20Gap%202020%20Technical%20Brief-Updated.pdf)


