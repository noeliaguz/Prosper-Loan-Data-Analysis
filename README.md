# Prosper Loan Data Analysis  

The goal for this project was to use various Python libraries (pandas, seaborn, matplotlib, etc.) to perform exploratory and explanatory data analysis on loan data from Prosper.
  
## Part I - Exploratory Analysis  
Python data science and data visualization libraries were used to explore the selected dataset’s variables and understand the data’s structure, oddities, patterns, and relationships. The analysis in this part is structured, going from simple univariate relationships to multivariate relationships. 

### Project Data

This data set contains 113,937 loans with 81 variables on each loan, including loan amount, borrower rate (or interest rate), current loan status, borrower income, and many others. The initial structure of the dataset can be inspected in [this data dictionary](https://docs.google.com/spreadsheets/d/1XMDjm5AUz4C-ZGZak465dK2mrSK-j4sF3cm0L6TbM-0/edit?usp=sharing) for the Prosper Loan data.  

The final structure of the dataset after variables of interest were selected:

 1. **listing_number** - The number that uniquely identifies the listing to the public as displayed on the website.

 2. **listing_creation_date** - The date the listing was created.

 3. **term** - The length of the loan expressed in months.

 4. **loan_status** - The current status of the loan: Cancelled,  Chargedoff, Completed, Current, Defaulted, FinalPaymentInProgress, PastDue. The PastDue status will be accompanied by a delinquency bucket.

 5. **borrower_apr** - The Borrower's Annual Percentage Rate (APR) for the loan.

 6. **listing_category_num** - The numeric category of the listing that the borrower selected when posting their listing: 0 - Not Available, 1 - Debt Consolidation, 2 - Home Improvement, 3 - Business, 4 - Personal Loan, 5 - Student Use, 6 - Auto, 7- Other, 8 - Baby&Adoption, 9 - Boat, 10 - Cosmetic Procedure, 11 - Engagement Ring, 12 - Green Loans, 13 - Household Expenses, 14 - Large Purchases, 15 - Medical/Dental, 16 - Motorcycle, 17 - RV, 18 - Taxes, 19 - Vacation, 20 - Wedding Loans

7. **borrower_state** - The two letter abbreviation of the state of the address of the borrower at the time the Listing was created.

8. **occupation** - The Occupation selected by the Borrower at the time they created the listing.

9. **employment_status** - The employment status of the borrower at the time they posted the listing.

10. **employment_status_duration** - The length in months of the employment status at the time the listing was created.

12. **debt_income_ratio** - The debt to income ratio of the borrower at the time the credit profile was pulled. This value is Null if the debt to income ratio is not available. This value is capped at 10.01 (any debt to income ratio larger than 1000% will be returned as 1001%).

12. **income_range** - The income range of the borrower at the time the listing was created.

13. **income_verifiable** - The borrower indicated they have the required documentation to support their income.

14. **loan_original_amount** - The origination amount of the loan.

16. **monthly_loan_payment** - The scheduled monthly loan payment.

17. **listing_category_name** - The name of the category of the listing that the borrower selected when posting their listing  
  
  
  
### Data Cleaning Steps

After selecting the variables of interest, the following clean up steps were be taken:

- Rename columns to make them lowercase and use underscores as separators
- Create a new column that lists the Listing Category Names (non-numeric)
- Adjust column data types: listing_creation_date (datetime)
- Drop rows according to time: Want to only pull data for the last 10 years, if applicable
- Convert income_range column to category, assign as ordinal category
- Round stated_monthly_income to 2 decimal places to better represent currency
- Final check for null values, decide if null values will be removed or adjusted
  
  
## Part II - Explanatory Analysis  


### Summary of Findings

> My main point of interest with this exploration was to find if the listing category variable had any bearing on loan status. While the listing category itself did not show correlation to loan status outcomes, it was discovered that about 56% of borrowers took out loans declared under the 'Debt Consolidation' category. Additionally, the mean Debt to Income Ratio (DIR) was found to be 27%, with most borrowers having a DIR ranging from 10-20%. Most borrowers also had income ranges from $25,000-49,999 and $50,000-74,999. Surprisingly, while 'Current' and 'Completed' loan statuses were more prevalent in the data, they also accounted for far greater data points than loans under 'Defaulted' and 'Chargedoff.' A positive point for both the borrowers and the company! These initial observations that led to further investigation of other variables that I wanted to find correlations with. 

> Once I began my biavariate exploration, I found some interesting initial interactions between the variables of interest. It was found that for the top 5 lising categories, the distribution of loans statuses was not as consistent, with Debt Consolidation accounting for more Current loans relative to other loan statuses, while the other 4 categories accounted for more Completed loans relative to the other loan statuses. Furthermore, among the loan statuses, the 'Defaulted' status had the highest average DIR with borrowers at 41%, with Cancelled at 34%, and Chargedoff at 33%. Something of note was observations made of the Employment Status variable. As it stands, the variable lists 'Employed,' 'Full-time,' and 'Part-time' as distinct categories without much reason as to why. I found that due to this ambuguity, using this variable in further investigations may be misleading.

> Moving into the multivariate exploration, I found the while there wasn't strong correlations to the listing category itself, other variables with the categories had interesting interactions. Initially, all listing categories were investigated, but this led to a sort of 'dead-end' as the visualization showed large error bars. This is due to there being not enough data to support certainty within those listing categories. Moving forward I was able to explore the average DIR for income ranges above zero listed by loan status within the top 5 listing categories. Interestingly, there was strong negative correlations between lower income ranges and average DIR. Furthermore, this correlation stayed consistent across the loan statuses, more particularly the Defaulted loan status, showing even higher DIRs comparitively.



### Key Insights for Presentation

> Listing Category: 56% of loans are categorized as Debt Consolidation

> Loan Status: The majority of the loans have a Current or Completed status. Surprisingly though, the difference between those and the Charged off or Default loans is greater than expected.

> DIR v Loan Status: 'Defaulted', 'Cancelled', and 'Chargedoff' loan statuses had higher DIRs (41%, 34%, and 33% average DIR, respectively) compared to the overall mean DIR (27%), as well as 'Completed' and 'Current' loans (26%)

> DIR v Loan Status v Listing Category v Income Range: Another correlation observed within the top 5 listing categories was between income range, average DIR, and loan status. There was a strong negative correlation between income range and average DIR. Borrowers within the 1-24,999 income range had higher DIRs on average. This then correlated to the even high average DIRs observed for that income range within the Defaulted loan status. Thus, this points to a third variable that has strong correlations to closed loan status outcomes.

> One conclusion for further investigation is to perform a further deep dive into the borrowers within the Debt Consolidation listing category. After cleaning the data, about 56% of borrower data came from that category alone! I believe by further inspecting correlations within this category, the company can better understand features that may predict how Current loans may result for the existing borrowers (as the majority of loans are under the 'Current' status). This can then lead to better decisions about how borrowers may be given acceptance or denial to loans provided by Prosper. 

> My suggestion for further improvement is to re-evaluate the 'Employment Status' variable. As it stands now, this variable is seemingly ambiguous. The reason for separating the statuses 'Full-time' and 'Part-time' vs 'Employed' not clear. Why were these statuses distinct? This made me hesitant to work with the variable as this could mean the data could be skewed with overlapping statuses. For instance, a borrower may have selected 'Employed' while also being a part-time worker. In this case, results for any of the statuses where overlap could occur may be skewed. Instead, it may provide more accuracy in the data to use the following Employment Status options for borrowers: Full-time, Part-time, Self-employed, Other, Retired, and Not employed. The removal of the general "Employed" status can more accurately contribute to the statistics and distribution of data within the Full-time and Part-time statuses.

