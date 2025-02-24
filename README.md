# Loan Dataset Project Summary (Feb 23, 2025)

## 🥅 Project Objective:

- This dataset contains 19 features with information regarding 111,107 loans. The 19 features capture information about the borrower and the loan. The loan status feature indicates whether loans were either Fully Paid, or Charged Off. Predicting which borrowers will pay off their loan, and which ones will default, is the lender's ultimate goal.

- Using this dataset, I hope to capture the indicators that best predict if a borrower will pay off their loan, or default.

## 📖 Directory
  [1. Exploratory Data Analysis](#exploratory-data-analysis)
  2. Preprocessing
  3. Explanatory Analysis
  4. Next Steps

## Exploratory Data Analysis
 - Each feature was explored for completeness, variability, inconsistencies, patterns, outliers, bad values, and anything else of significance.

  1. **Loan ID:** every loan has a unique loan ID, so this feature should contain only unique values. However, there were 22,197 duplicates that need to be addressed. After dropping all duplicates where the rows matched 100%, there were still more duplicates within the Loan ID column. These duplicates will be addressed throughout the rest of the EDA process, and by the end of data cleaning, no duplicates remained. 
  2. **Customer ID** one customer may have multiple loans, so we do not require the customer ID's to be unique. However, by the end of EDA, all customer ID's are unique, which means that in this dataset, each customer only borrowed 1 loan.
  3. **Loan Status** either Fully Paid or Charged Off, this is the target of the model.
  4. **Current Loan Amount** 

## 🧮 Preprocessing
  1. **Loan ID:**  the Loan ID's hold no inherent meaning, and is unrelated to whether a Loan will default or not, so this feature is dropped.
  2. **Customer ID:** the Customer ID's also do not contain any meaningful information related to the status of the loan, so this feature is also dropped.
  3. **Loan Status** as the target of the model, Loan Status is separated from the rest of the features during the train/test/split.
  4. 

## 📈 Explanatory Analysis
 


## 🥾 Next Steps

- Rather than relying on the RobustScaler to address all the outliers, see if removing them via a standard deviation-based threshold will improve model accuracy.
- Explore alternatives to the decisions made during preprocessing, to see how if model accuracy can be improved by changing my assumptions.
- Drop highly correlated features. For example, between Tax Liens and Bankruptcies, drop one of them.
