# SAS-Project-A-Data-Driven-Approach-to-Optimizing-Store-Locations
## Strategic Expansion Analysis for ABC Cannabis Group
## Table of Content

- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools](#tools)
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)
- [Correlation Analysis](#correlation-analysis)
- [Outcomes](#outcomes)
- [Key Model Variables](#key-model-variables) 
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Model Report](#model-report)
- [Data Analysis](#data-analysis)
- [Key Findings](#key-findings)
- [Limitations](#limitations)
- [Code](#code)
- [References](#references)

## Project Overview
My primary objective is to design a data-driven approach that will not only identify promising areas for store openings but also provide insights into the underlying rationale behind each recommendation.
I expect my model to account for variables such as population density, income levels, and consumer behavior patterns.
In addition, I expect my developed model to be scalable and adaptable, allowing for adjustments based on any evolving market dynamics. 
Ultimately, the goal is to empower ABC Cannabis Group with actionable insights that support informed decision-making in their expansion efforts, leading to sustainable growth and competitive advantage within the industry.

## Data Source
Consumer Research Report: The primary datasets used for this analysis is the "dev.csv" and "val.csv" file, which contains consumer demographics and transaction data.

## Tools
- SAS Studio - for data cleaning, analysis and visualisation.
- Microsoft Power Point - for presentation.
- Microsoft Excel - for visualisation.

## Data Cleaning and Preparation
In preparing my data for analysis I performed the following tasks:
1. Importing of data into SAS Studio.
2. Dropped variables not needed for exercise.
3. Identified target variables & Performed basic statistics.
4. Performed random sampling of analytical file to confirm data integrity & gives a small, interpretable snapshot of the dataset.

## Correlation Analysis
A correlation analysis was run with total spent as the target variable against a number of predictors. The idea was to prioritize predictors based on their relationship with spending (TOT__SPENT7), focusing on the most impactful features.

## Outcomes
At a 5% significance level, 111 predictors were statistically significant. With 22 showing a strong relationship with the target variable (total spent).
42 predictors were not statistically significant and almost showed no relationship with the target variable.

## Key Model Variables
In determining my key model variables I considered the first 20 highly correlated predictors with the target variable and runned a series of stepwise to arrive at the final model variables.

## Exploratory Data Analysis
I performed EDAs to visualizes and interprets how different predictors influence spending behavior across ranges.
- Penchant for High Risk
- Hosehold Population 19-34
- Total Hosehold Population
- Hosehold Population 19+
- Total Number of Children at Home
- Total Spent on Clothing
- Total Spent on Property Taxes

## Model Report
Penchant(SV00058) for high risk explains a significant portion of the variance in total spent. It has a positive effect on the model with a high partial r-squared value of 0.7879, which shows that it is a strong predictor within the model. The extremely low p-value and high F-value strongly suggest that this variable is statistically significant and critical driver in the model.

Home language Panjabi(ECYHOMPANJ) is the second variable which contributes an additional 4.02% to the model. The commulative r-squared increases to 0.823, indicating its useful explanatory power to the model despite being much less influential than Penchaant(SV00058).

Household Population 19-34(CNBBAS1934) the third variable adds another 2.87% to the explanation of the dependent variable's variance. This brings the models r-squared to 0.8482 strengthen the models overall explanatory power.

Spent on Home Entertainment Equipment and Services(HSRE040), contribution to the model 1.91% raising the r-squared to 0.8648. While each additional variable contributes less incrementally, they all positively impact the models predictive capabilities.

Men Aged 15+ spend on jewelry(HSCM001F) contributes the least (1.05%) to the model. Interestingly it has a negative impact, which suggests men aged 15 and above who spend on jewelry(HSCM001F) inversely affects total spent on cannabis. Inspite of this its inclusion on the model improves the models r-squared to 0.874.










