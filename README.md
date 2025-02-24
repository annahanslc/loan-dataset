# Loan Dataset Project Summary (Feb 23, 2025)

## 🥅 Project Objective:

- This dataset contains 19 features with information regarding 111,107 loans. The 19 features capture information about the borrower and the loan. The loan status feature indicates whether loans were either Fully Paid, or Charged Off. Predicting which borrowers will pay off their loan, and which ones will default, is the lender's ultimate goal.

- I aim to use this dataset to generate a model that helps to predicts if a borrower will pay off their loan.

## 📖 Directory
1. [Exploratory Data Analysis](#-exploratory-data-analysis)
2. [Preprocessing](#-preprocessing)
3. [Explanatory Analysis](#-explanatory-analysis)
4. [Next Steps](#-next-steps)
  

## 🧭 Exploratory Data Analysis
 - Each feature was explored for completeness, variability, inconsistencies, patterns, outliers, bad values, and anything else of significance.

  1. **Loan ID:** every loan has a unique loan ID, so this feature should contain only unique values. However, there were 22,197 duplicates that need to be addressed. After dropping all duplicates where the rows matched 100%, there were still more duplicates within the Loan ID column. These duplicates will be addressed throughout the rest of the EDA process, and by the end of data cleaning, no duplicates remained. 
  2. **Customer ID** one customer may have multiple loans, so we do not require the customer ID's to be unique. However, by the end of EDA, all customer ID's are unique, which means that in this dataset, each customer only borrowed 1 loan.
  3. **Loan Status** either Fully Paid or Charged Off, this is the target of the model.
  4. **Current Loan Amount** contained 12,738 instances of 99,999,999, which is a bad value. These observations are dropped from the dataset.
  5. **Term** 66,535 observations were Short Term, while 25,965 observations were Long Term.
  6. **Credit Score** credit scores should not exceed 800, however, there were 5,044 observations where the credit scores were above 5,000. These inconsistent values appeared to arise from an error, where an extra 0 was added to the end, amplifying the values by 10. This is can be rectified by dividing all values above 5,000 by 10. After making this correction, there are still 21,338 observations with null values in this feature. Credit Score is designed to signal credit worthiness, and is a critical piece of the model. Any imputing of these null values, roughly 30% of all observations, could greatly sway the model. Dropping these nulls will still leave 71,162 datapoints in the dataset. Although neither option is desirable, dropping the nulls is the better option in this situation.
  7. **Years in current job** the values range from "< 1 year" to "10+ years", with a total of 11 categories. There are also 3,069 null values. Assuming that null values indicate "no job", a 12th category was added to the range.
  8. **Home Ownership** the 3 valid categories are "Rent", "Home Mortgage" and "Own". However, there are 145 observations of "HaveMortgage", which seems to be a mislabeling of "Home Mortgage". Once corrected, 3 nominal categories were confirmed.
  9. **Annual Income** the values are spread out, with extreme outliers. To reduce the effect of outliers, while preserving the underlying distribution of income, outliers are dropped via a threshold based on the standard deviation.
  10. **Purpose** contains 16 categories, and several that overlap with each other. Vast majority of the datapoints are under one single category, "Debt Consolidation". Combined the 15 non-"Debt Consolidation" categories into one category to allow the model to find more meaningful patterns. Transfromed the feature into a binary column indicating whether or not the purpose of the loan is for debt consolidation.
  11. **Montly Debt** feature contains both floats and strings. Cleaned the strings of any non-numeric characters, then converted all values to floats. The resulting distribution shows large outliers, and these will be addressed by the RobustScaler in preprocessing.
  12. **Years of Credit History** appears to be normally distributed, with no missing values.
  13. **Months since last delinquent** 52% of the dataset is missing values in this feature. It is assumed that the null values may mean that the borrower was never delinquent. This is indicative of the borrower's credit worthiness. To capture this information, feature was modified as "Ever Delinquent" where 0 means never delinquent, and 1 means was delinquent at some point in time.
  14. **Number of Open Accounts** Skewed to the right, with some outliers.
  15. **Number of Credit Problems** vast majority of values are 0, while the rest range from 1-15, with only 1 or 2 datapoints in the higher numbers. To help the model find meaningful pattern in this feature, transform feature into whether or not the borrower has had any credit problems. Where 0 means they do not have any credit problems, and 1 means they have 1 or more credit problems.
  16. **Current Credit Balance** distribution shows a number of outliers. Addess with RobustScaler.
  17. **Maximum Open Credit** there are 31,457 observations that are strings. Convert invalid values to nulls and then convert strings to numeric. Impute nulls using the median. Distribution contains large outliers, address with RobustScaler.
  18. **Bankruptcies** ranging in values from 0 to 6, majority are 0. To reduce cardinality, combine all categories 1 or more into the same category. Transform feature into a binary indicator of if the borrower had ever been bankrupt before.
  19. **Tax Liens** ranging in values from 0 to 15, again, majority of values are 0. Also transform this feature into a binary indicator of ever having a Tax Lien or not. 

## 🧮 Preprocessing
  1. **Loan ID:**  the Loan ID's hold no inherent meaning, and is unrelated to whether a Loan will default or not, so this feature is dropped.
  2. **Customer ID:** the Customer ID's also do not contain any meaningful information related to the status of the loan, so this feature is also dropped.
  3. **Loan Status** as the target of the model, Loan Status is separated from the rest of the features during the train/test/split.
  4. **Current Loan Amount** after dropping the bad values, the distribution appears to be normal, with a right skew. Use StandardScaler for scaling.
  5. **Term** as a binary feature, Term is one hot encoded, with second column dropped.
  6. **Credit Score** after fixing inconsistent data and removing nulls, distribution appears to be mostly normal. Use StandardScaler for scaling.
  7. **Years in Current Job** use Ordinal Encoder to encode the 12 categories in order from 0 to 11.
  8. **Home Ownership** use One Hot Encoder to encode the 3 nominal categories
  9. **Annual Income** to prevent data leakage, a custom transformer is created to calculate the standard deviation and mean based on the train data, and then used to transform the test and val dataset. Use RobustScaler for scaling.
  10. **Purpose** use One Hot Encoder to encode the binary feature, and drop second column.
  11. **Monthly Debt** use the RobustScaler to reduce the effect of outliers.
  12. **Years of Credit History** apply StandardScaler
  13. **Months since last delinquent** use One Hot Encoder to encode the binary feature, and drop second column.
  14. **Number of Open Accounts** apply RobustScaler to reduce the effect of outliers.
  15. **Number of Credit Problems** use One Hot Encoder to encode the binary feature, and drop second column.
  16. **Current Credit Balance** apply RobustScaler to reduce the effect of outliers.
  17. **Maximum Open Credit** impute invalid values with the median using SimpleImputer. Then, apply RobustScaler to reduce the impact of outliers.
  18. **Bankruptcies** use One Hot Encoder to encode the binary feature, and drop second column.
  19. **Tax Liens** use One Hot Encoder to encode the binary feature, and drop second column.

## 📈 Explanatory Analysis
 ![LoanStatusDashboard](https://github.com/user-attachments/assets/021778a7-7786-4dad-bdcf-984014f8e0fb)

The charts above display Loan Status from the perspective of different features, with the goal of visualizing any patterns or correlations. Blue portions indicate loans that defaulted, while Orange indicate loans that were fully paid off. 

The top left diagram show loan status along the range of "Months since last delinquent". The assumptions was that the more recent a borrower was delinquent on their payment, the more likely that they would default on their loan. The resulting chart, however, does not show any obvious increase in the portion of loans that defaulted on the left end of the x-axis. This seems to indicate that, contrary to my assumption, there is a not a significant correlation between the number of months since last delinquent with Loan Status. However, in EDA, it was determined that Months since last Delinquent would be transformed into the binary feature "Ever Delinquent", this feature may reveal additional information. 

The top right diagram depicts loan status for different number of open accounts. Again, there does not appear to be much difference in the portion of defaulted loans at each step of the histogram.

The lower left diagram shows loan status based on the purpose of the loan. Debt Consolidation is by far, the most common purpose for the loan. It also appears that the portion of defaulted loan is the highest for this category. During pre-processing, this feature was transformed into binary where it indicates if the loan was for Debt Consolidation or not. This new feature may reveal additional insight.

The lower middle diagram creates a two-dimensional plane between Tax Liens and Bankruptcies. As we know that these two features are correlated, it amplifies the effect of "Poor Financial History" on loan status. We can clearly see that Blue portion (defaulted) increases on the ends of the axes. This means that both are positively correlated with loan default.

The lower right chart show increasing years of credit history, and the portion of defaulted loans on that spectrum. There does seem to be a general trend of lower rates of loan default as the number of years of credit history increases.


## 🥾 Next Steps

- Rather than relying on the RobustScaler to address all the outliers, see if removing them via a standard deviation-based threshold will improve model accuracy.
- Explore alternatives to the decisions made during preprocessing, to see how if model accuracy can be improved by changing my assumptions.
- Drop highly correlated features. For example, between Tax Liens and Bankruptcies, drop one of them.
