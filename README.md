# LTFS_AnalyticsVidhya
This repository contains our team's[Fools]([Sanchit Singh](https://bit.ly/2JAuK9l) & me) solution for the [LTFS Data Science FinHack](https://datahack.analyticsvidhya.com/contest/ltfs-datascience-finhack-an-online-hackathon) (ML Hackathon) organized by Analytics Vidhya.

## Problem Statement 
Financial institutions incur significant losses due to the default of vehicle loans. This has led to the tightening up of vehicle loan underwriting and increased vehicle loan rejection rates. The need for a better credit risk scoring model is also raised by these institutions. This warrants a study to estimate the determinants of vehicle loan default. A financial institution has hired you to accurately predict the probability of loanee/borrower defaulting on a vehicle loan in the first EMI (Equated Monthly Installments) on the due date. Following Information regarding the loan and loanee are provided in the datasets:

    Loanee Information (Demographic data like age, income, Identity proof etc.)
    Loan Information (Disbursal details, amount, EMI, loan to value ratio etc.)
    Bureau data & history (Bureau score, number of active accounts, the status of other loans, credit history etc.)

Doing so will ensure that clients capable of repayment are not rejected and important determinants can be identified which can be further used for minimizing the default rates.

## Data Description
 - train.zip contains train.csv and data_dictionary.csv.

 - train.csv contains the training data with details on loan as described in the last section

 - data_dictionary.csv contains a brief description on each variable provided in the training and test set.

 - test.csv contains details of all customers and loans for which the participants are to submit probability of default.

 - sample_submission.csv contains the submission format for the predictions against the test set. A single csv needs to be submitted as a solution.
 
## Evaluation Metric
Submissions are evaluated on area under the ROC curve between the predicted probability and the observed target.

## Public and Private Split
Test data is further randomly divided into Public (25%) and Private (75%) data. The final rankings would be based on your private score which will be published once the competition is over.

## Leaderboard
 - [Public LB Score & Rank](https://datahack.analyticsvidhya.com/contest/ltfs-datascience-finhack-an-online-hackathon/lb) -  	0.6652878571 & 26/1340
 - [Private LB Score & Rank](https://datahack.analyticsvidhya.com/contest/ltfs-datascience-finhack-an-online-hackathon/pvt_lb) -  	0.6694836578 & 15/1339
 
 ## Approach
 
 ### Validation Strategy
 The first step to any Data Science problem must be setting up a reliable cross-validation strategy. We tested both 5 fold Stratified split & 5 fold Time Series Split on Disbursal Date. The Stratified KFold strategy provided better results with adding features so we used that for the rest of the competition.
 
 ### Features
  - Worked 
    - Asset Cost / Disbursed Amount and log1p transform of it
    - Binning of CNS Score Description into Low Risk, High Risk and no history and not scored
    - Difference between Primary Sanctioned Amount and Primary Disbursed Amount
    - log1p transform of LTV
    - Difference between Sanctioned Amount (Primary + Secondary) and Disbursed Amount (Primary + Secondary)
  - Not Worked
    - log1p transform of various numerical column
    - Clipping of Features having less unique values
    - Combined Id using different ids
    - Various features from age at Disbursal Date
    - Counts of Employee Code
    - Various Total features combining Primary and Secondary accounts
   
   Features were selected if they improved the local cross validation score of CatBoost model
   
### Final Submission 
Blending on the same features that worked using CatBoost, XGBoost and LightGBM models tuned using Scikit-Optimize.

``Final Sub = 0.7 * (CatBoost) + 0.15 * (XGBoost) + 0.15 * (LightGBM)``
