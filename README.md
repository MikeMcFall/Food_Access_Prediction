# Food Access Prediction
I formulated and worked on this project as part of Springboard's Data Science Career Track program.  

Grocery stores and convenience stores are not equally distributed in counties in the United States. Grocery stores typically sell a wide
variety of foods and household goods such as fresh fruits, vegetables, and meat as well as canned and prepared foods. Convenience stores 
normally only carry a limited variety of prepared foods at a higher cost than grocery stores. The unequal distribution of these means that 
some areas will not have as easy access to fresh and/or cheaper foods available at grocery stores. In this project I will create a model 
using socioeconomic variables to predict the ratio of convenience to grocery stores in a county. A high value for this ratio likely means 
that it is more difficult to access a grocery store.

## Data Source
For this project, I identified three datasets which contained variables of interest:  
1. The United States Department of Agriculture’s <a href='https://www.ers.usda.gov/data-products/food-environment-atlas/data-access-and-documentation-downloads/'>Food Access Research Atlas</a>  
    1. Supplemental  Data - County  
        1. FIPS (A unique identifier for each county)  
        1. County  
        1. State  
        1. Population Estimate, 2015  
    1. STORES  
        1. FIPS  
        1. GROCPTH14 (Number of grocery stores per thousand population)  
        1. SUPERCPTH14 (Number of supercenter stores per thousand population)  
        1. CONVSPTH14 (Number of convenience stores per thousand population)
    1. SOCIOECONOMIC  
        i. Kept all variables
1. <a href='https://factfinder.census.gov/faces/nav/jsf/pages/download_center.xhtml'>The American Community Survey’s</a> 5-year estimates of educational attainment  
    1. GEO.id2 (FIPS)  
    1. HC_02_EST_VC17 (% of population high school graduate or higher)  
    1. HC_02_EST_VC18 (% of population bachelor's degree or higher)  
1. The Internal Revenue Service’s <a href='https://www.irs.gov/statistics/soi-tax-stats-county-data-2015'>Statistics of Income</a>  
    1. FIPS (columns 0 & 2)  
    1. agi_stub (The Adjusted Gross Income Bracket numbered 1-8)  
       1.    1 = Under $1  
       1.   2 = $1 under $10,000  
       1.  3 = $10,000 under $25,000  
       1.   4 = $25,000 under $50,000  
       1.    5 = $50,000 under $75,000  
       1.   6 = $75,000 under $100,000  
       1.  7 = $100,000 under $200,000  
       1. 8 = $200,000 or more  
    1. N1 (The number of returns filed in a county in an income bracket)  

## Data Wrangling
### Merging Datasets
The first two datasets were were ready to be merged on the FIPS county code. The IRS data had a column for the state FIPS and
one for the county FIPS. These needed to be combined to create the same values as the other datasets. The IRS data also needed
to be pivoted from containing 8 rows per county to 1 row.
### Missing Data
There were two rows missing all of the USDA data and one row missing the IRS data; all three were removed from the analysis.
There are 67 rows where conv_to_groc is missing because there are zero grocery stores in the county and dividing by zero is 
undefined. Those where the number of convenience stores is also zero were set to 0. One good predictor of this ratio is 
median household income. To fill the rest of the missing values, I created 20 bins of the income variable and used the 
maximum ratio in each bin.

## Machine Learning
I split the datset into three parts:  
- 70% for training  
- 15% for testing while trying different models  
- 15% for final validation after all hyper-paramters were chosen  
I then trained many regression model from a simple linear regression to using gradient boosting. The gradient boosting regressor 
had the lowest RMSE on the test set. I then used a cross-validated grid search to tune the hyper-paramters. The final model performed
similarly on the validation and test sets.
