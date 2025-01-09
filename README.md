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
- [Regression Model](#regression-model)
- [Multiple Regression](#multiple-regression)
- [Model Performance and Analysis](#model-performance-and-analysis)
- [Interpretation](#interpretation)
- [Conclusion](#conclusion)
- [Limitations](#limitations)
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

## Code
~~~sas
*Reading dev file into SAS;
options validvarname= v7;
proc import out= dev replace
datafile= '/home/u63586186/BAN210/ban210dev1.csv' dbms= csv;
guessingrows= 500;
getnames= yes;
run;

*Reading val file into SAS;
options validvarname= v7;
proc import out= val replace
datafile= '/home/u63586186/BAN210/ban210val1.csv' dbms= csv;
guessingrows= 500;
getnames= yes;
run;

*Drop variables not needed for exercise;
data dev1;
set dev;
drop PRIZM5DA SG LS DensityClusterCode15 DensityClusterCode15_2 SG_U2
     DensityClusterCode5_lbl DEPVAR7;
run;     

*Basic statistics of target and independent variables;
title 'Basic Stat of Target Variable & 5 Independent Variables'; 
proc means data= dev1 N mean median min max std;
var TOT__SPENT7 CNBBAS1934 SV00058 HSCL001 HSGC001 ECYHTA6064;
run; 

*Random Sample of analytical file;
proc surveyselect data= dev1 
out= randev /* saving to randev1 */
method= srs /* simple random sampling */
n= 10 /*number of records to select */
seed= 12345;
run;
title 'Ten Random Records from Analytical file with Target & Predictors';
proc print data= randev (keep= TOT__SPENT7 CNBBAS1934 SV00058 HSCL001 HSGC001 
                               ECYHTA6064 DensityClusterCode5);
run;
~~~

## Correlation Analysis
A correlation analysis was run with total spent as the target variable against a number of predictors. The idea was to prioritize predictors based on their relationship with spending (TOT__SPENT7), focusing on the most impactful features.

## Code
~~~sas
proc corr data= dev1 rank out= corep;
with tot__spent7;
run;
proc transpose data= corep out= out1;
data out (keep= _name_ snp tot__spent7);
set out1;
snp= abs(tot__spent7);
proc sort data= out;
by descending snp;
proc print data= out;
run;
~~~

## Outcomes
At a 5% significance level, 111 predictors were statistically significant. With 22 showing a strong relationship with the target variable (total spent).
42 predictors were not statistically significant and almost showed no relationship with the target variable.

## Code
~~~sas
*Running first stepwise;
proc stepwise data= dev1;
model tot__spent7= SV00058 CNBBAS1934 ECYBASHPOP CNBBAS19P ECYCHAKIDS CNBBAS35P HSCL001
                   HSGC001 HSHC001 ECYHOMPANJ HSRE001 HSRE011 HSRE040 HSRO001
                   HSSH014 ECYHTA6064 HSCM001D HSCM001F HSRE001S WSD2AR ECYHTA3034
                   ECYHTA5559 ECYMTN2534 ECYHTA6569 SV00044 ECYTIMSA SV00066 HSED006
                   ECYHTA2529 HSTR050 SV00041 HSCS013 ECYMTN3544 SV00021 ECYHTA7074
                   ECYSTYSING ECYSTYAPT HSCS007 ECYTENOWN HSRE061 HSRO002 SV00093 SV00086
                   HSSH037B ECYMARSING HSTA002A ECYCDOIC HSTA005 SV00043 WSIN100_P ECYHSZ2PER
                   WSWORTHV ECYPIMNI ECYRELCHR SV00079 SV00030 SV00028 ECYTRAWALK HSCS008
                   SV00011 HSRE021 HSRV001B SV00038 SV00061 ECYRELCATH HSRM014 ECYOCCSCND
                   ECYMARM HSHC007 SV00012 ECYACTINLF ECYMARWID ECYHSZ1PER HSTA002B HSRE006
                   HSTR034 HSSH037A HSTA006 HSHC004B ECYACTUR HSHC003 HSMG008 ECYCFSLP HSRE063
                   SV00025 ECYINDADMN ECYTRAPUBL ECYPOC17P HSHE012 ECYPOWHOME SV00002 ECYTIMSAM 
                   HSRE052 ECYINDARTS SV00023 ECYHOMUKRA HSRE042 ECYOCCMGMT 
                   ECYINDMANU HSHC001S ECYINDEDUC ECYSTYAPU5 DensityClusterCode5 WSCARDSB ECYHOMCHIN 
                   ECYMARCL HSED005 SV00064 SV00077 SV00074 ECYHOMFREN/
sls=.05 sle=.05;
run;

*Running second stepwise;
proc stepwise data= dev1;
model tot__spent7=
SV00058
ECYHOMPANJ
CNBBAS1934
HSRE040
HSCM001F/
sls=.05 sle=.05;
run;
~~~

## Key Model Variables
In determining my key model variables I considered the first 20 highly correlated predictors with the target variable and runned a series of stepwise to arrive at the final model variables.

## Exploratory Data Analysis

## Code
~~~sas
*EDA's on correlation from key variables Sign;
data edarep;
set out;
_N= left(_N_);
call symput('a'!!_N,_NAME_) ;
run;
%macro contin(dataset=,GR=,VAR=);
data tfile;
set &dataset;
count= 1;
keep &var TOT__SPENT7;
proc rank data= tfile group= &GR OUT= tfile1;
var &var;
ranks __rank;
run;
proc summary data= tfile1 noprint;
class __rank;
var &var  TOT__SPENT7;
output out= tfile2
min= minvar DU1
max= maxvar DU4
mean= DU7 defect2
n= Count du2;
run;
data tfile;
informat mminvar $8. mmaxvar $8.;
length _var $20. range $20.;
set tfile2;
_var = "&VAR" ;
*_LAB = "&LAB" ;
mminvar= put(MINVAR,8.2);
mmaxvar= put(MAXVAR,8.2);
range= MMINVAR||' to '||MMAXVAR;
if __rank= 0 then Range=' <= '|| MMAXVAR;
if __rank= &GR-1 then range= MMINVAR ||'+';
if minvar = maxvar	then range= MMINVAR ;
if _N_ = 1 then Range = 'Average';
pcdef = defect2;
label Range='Range'
Count= 'Count'
pcdef= 'Average Spent';
proc print split= ' ';
var _var Range  pcdef  COUNT;
run;
%mend;
options missing= ' ';
%macro eda;
%do i=150 %to 153;
%contin(dataset= dev1,GR=5,VAR=&&a&i)	;
run;
%END;
%mend eda;
%eda;
~~~

<img width="1560" alt="Screenshot 2025-01-08 at 11 42 03 PM" src="https://github.com/user-attachments/assets/2eea8623-c1e4-4824-8bbb-e2583061f8bd" />

I performed EDAs to visualizes and interprets how different predictors influence spending behavior across ranges.
- Penchant for High Risk
- Hosehold Population 19-34
- Total Hosehold Population
- Hosehold Population 19+
- Total Number of Children at Home
- Total Spent on Clothing
- Total Spent on Property Taxes

<img width="1565" alt="Screenshot 2025-01-08 at 11 44 41 PM" src="https://github.com/user-attachments/assets/ed53e37b-8d20-45e6-a725-09bd80b84426" />

## Model Report

<img width="738" alt="Model Variable Report" src="https://github.com/user-attachments/assets/48743deb-af7d-4134-97fc-ac41a160ee8a" />

Penchant(SV00058) for high risk explains a significant portion of the variance in total spent. It has a positive effect on the model with a high partial r-squared value of 0.7879, which shows that it is a strong predictor within the model. The extremely low p-value and high F-value strongly suggest that this variable is statistically significant and critical driver in the model.

Home language Panjabi(ECYHOMPANJ) is the second variable which contributes an additional 4.02% to the model. The commulative r-squared increases to 0.823, indicating its useful explanatory power to the model despite being much less influential than Penchaant(SV00058).

Household Population 19-34(CNBBAS1934) the third variable adds another 2.87% to the explanation of the dependent variable's variance. This brings the models r-squared to 0.8482 strengthen the models overall explanatory power.

Spent on Home Entertainment Equipment and Services(HSRE040), contribution to the model 1.91% raising the r-squared to 0.8648. While each additional variable contributes less incrementally, they all positively impact the models predictive capabilities.

Men Aged 15+ spend on jewelry(HSCM001F) contributes the least (1.05%) to the model. Interestingly it has a negative impact, which suggests men aged 15 and above who spend on jewelry(HSCM001F) inversely affects total spent on cannabis. Inspite of this its inclusion on the model improves the models r-squared to 0.874.

## Regression Model

## Code
~~~sas
*Run multiple regression;
proc reg data= dev1 outest= regout1;
oxyhat: model tot__spent7= SV00058
ECYHOMPANJ
CNBBAS1934
HSRE040
HSCM001F;
run;
~~~

A regression model was built using proc reg, this involves fitting a multiple regression equation to predict spending (TOT__SPENT7) by leveraging key predictors (SV00058, ECYHOMPANJ, CNBBAS1934, HSRE040, HSCM001F), with the purpose of creating a statistically sound and interpretable model that captures the relationship between spending and its most influential factors.

<img width="950" alt="Screenshot 2025-01-08 at 11 52 03 PM" src="https://github.com/user-attachments/assets/a63c8735-963e-4597-8145-bb603763416e" />

## Multiple Regression

The validation dataset was scored using proc score to apply the regression model to unseen data (val) to generate predicted spending values (oxyhat). This ensures the dataset is clean by addressing missing values, and prepares it for segmentation analysis, with the primary purpose of assessing the model’s accuracy, generalizability, and reliability in predicting spending patterns in a new context.

## Code
~~~sas
*Running algorithm against validation dataset;
proc score data= val
score= regout1 out= rscorep
type= parms;
VAR SV00058
ECYHOMPANJ
CNBBAS1934
HSRE040
HSCM001F;
run;
~~~

## Model Performance and Analysis

Decile analysis was done to segment the validation dataset into 10 groups based on predicted spending scores, enabling the examination of spending and account distributions within each group. Key metrics like average spending and the percentage contribution to total spending were calculated for each decile, helping identify high-value regions (top deciles) for potential focus in expansion efforts, while also revealing diminishing returns in lower deciles. Following this, the gains and lift analysis calculates cumulative spending and lift metrics across the deciles, which are then formatted and visualized to provide insights into the model’s effectiveness.

## Code
~~~sas
data file2;
set rscorep;
array raw _numeric_;
do over raw;
if raw= . then raw= 0;
end; 
run;
*sortting validation file by model score in descending order and putting it into 10 deciles;
proc sort data= file2;
by descending oxyhat;
data file3;
set file2 nobs= no;
account= 1;
if _N_= 1 then  percent= 0;
if ceil((_N_/NO)*10) gt percent then  percent + 1;
retain percent;
run;
proc summary data= file3;
class percent;
var  oxyhat tot__spent7 account;
output out= test3 SUM= DU1 totspend ACCOUNT 
mean=DU2 SPENDRATE du3
min=SCORE du4 DU5;
DATA GAINS1;
SET TEST3;
DROP DU1-DU5 _TYPE_ _FREQ_;
IF _N_=1 THEN DO;
TOTALSPEND= totspend;
TOTSP= SPENDRATE;
cummail= 0;
END;
ELSE DO;
PCTOTSP=ROUND((TOTSPEND/TOTALSPEND)*100,.01);
INRSRATE=ROUND((spendrate/TOTSP)*100,.01);
END;
RETAIN TOTALSPEND TOTSP;
DATA GAINS2;
SET GAINS1;
IF _N_=1 THEN DELETE;
LABEL SCORE='MINIMUM* SCORE IN RANGE'
PCTOTSP='% OF TOTAL* SPEND IN INTERVAL'
SPENDRATE='AVG. SPEND. RATE* WITHIN INTERVAL'
INRSRATE='INTERVAL LIFT IN SPEND.RATE'
percent='% OF PROSPECTS *IN INTERVAL';
PROC FORMAT;
value  ABC 1='0-5%'
                  2='5%-10%'
                  3='10%-15%'
                  4='15%-20%'
                  5='20%-25%'
                  6='25%-30%'
                  7='30%-35%'
                  8='35%-40%'
                  9='40%-45%'
                  10='45%-50%'
                  11='50%-55%'
                  12='55%-60%'
                  13='60%-65%'
                  14='65%-70%'
                  15='70%-75%'
                  16='75%-80%'
                  17='80%-85%'
                  18='85%-90%'
                  19='90%-95%'
                  20='95%-100%';
				 
PROC PRINT DATA=GAINS2 SPLIT='*';
ID PERCENT;
VAR score pctotsp spendrate ACCOUNT;
*FORMAT PERCENT ABC.;
title "Development - Multiple Regression";
quit;
~~~

<img width="499" alt="Gains Chart" src="https://github.com/user-attachments/assets/40bab432-58ae-4b35-899e-98c9735d08f4" />

## Interpretation

The first decile captures a significant portion of the total spending on cannabis in the past month, indicating that targeting these areas will yield substantial profitability.
The second decile still shows high potential but with a significant drop in spending percentage to 14.83%, indicating diminishing returns as scores decrease.
The trend continues with each lower decile showing progressively smaller shares of total spending and average spending rates.
Minimum score in range (-3532.87) indicates areas with lower reliability scores. This decile demonstrates the least potential and thus will be less attractive for store placements.

## Conclusion

### Prioritise High-Value Areas
Tha gains chart highlights 10% of areas by predicted score contributing to overall spending, this indicates that opening stores in high-scoring areas is likely to yield the best return on investment(ROI)

### Cautious Approach to Low Deciles 
As the predictive score decreases, so does the proportional spending. Suggesting that the lower deciles represent areas with lower economic potential for new stores. However it might be beneficial to consider other qualitative factors.

### Resource Allocation
The differentiation in spending and interval distribution across deciles can guide budget allocations not just for store setups but also for operational expenses such as staffing, inventory management and promotional activities.

### Dynamic Market Scoring 
Updating the predictive model perodically to reflect changing market conditions and consumer behavior can help ABC Cannabis Group stay adaptive and responsive to market dynamics.

### Pilot Projects 
Before fully committing to opening new stores in high-scoring areas, conduct pilot projects to test market assumptions. This might include temporary pop-up stores or targeted marketing campaigns to gauge actual customer response.

## Limitations
1. External Factors: The model does not account for sudden market changes or external factors like economic shifts, competition, or local events, which could affect spending patterns in certain areas.
2. Predictor Constraints: The model’s accuracy is limited by the predictors chosen. Some relevant factors influencing spending might be overlooked or not included in the analysis.
3. Scalability: Although the model is designed to be adaptable, scaling it for different geographic regions or varying market conditions could require significant adjustments or re-validation.
4. Overfitting Risk: While the model attempts to find the best predictors, there’s a risk of overfitting to the available data, meaning it might perform well on the current dataset but may not generalize as effectively to new data.
5. Decile Grouping: The decile segmentation method may not fully capture niche markets or outliers that could represent high-value customers in smaller groups.

## References

1. Data Mining for Managers by Richard Boire.
2. Learning SAS by Example 2nd Edition by Ron Cody.
