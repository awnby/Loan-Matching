# Loan-Matching
### Case Study Prompt
Let's imagine a loan matcher has 3 lending partners, “A”, “B”, and “C”. Each lender is providing the same offer, but, for each approval, lender A provides us $250, lender B pays us $350, and lender C pays us $150. 
For 100K of our customers, we have collected a set of data in hopes to understand what variables are most important in determining approvability. We believe if we collect the right information, we will be able to show the appropriate lender to each customer to maximize approval rate and revenue. The company wants to know if they can increase revenue per application by matching certain groups of customers to specific lenders. 

##### Explore the variables relationship with approvability:
Possible things to consider: Which variables are the most helpful in understanding if a customer is going to be approved or denied for a loan? Are there certain variables that are not useful to collect? Are there any feature modifications or transformations that would improve the predictive power of a variable?
##### Tell us about the lenders approval rates:
Possible things to consider: What is each Lender’s average approval rate? Are there any clear differences between the three different lenders on what type of customers they approve? Are there variables that reliably predict a user’s approval likelihood for a particular lender?
##### Evaluate which customers we should match to each lender to maximize Revenue Per Application:
Calculate how much incremental revenue we could make if we matched lenders to certain groups of customers more appropriately.
Possible things to consider: Are there groups of customers that would be a better fit for a different lender? What considerations should we have in mind if we planned to match customers with lenders in real time?

### Data Dict
User ID - unique ID to represent the customer who submitted the application (string)
Application - count of application - each row is 1 application submitted (int)
Reason - the purpose for requesting a loan (string)
Loan Amount - the loan value size (int)
FICO Score - score used to make credit risk decisions (int)
FICO Score Group - categorical grouping of FICO scores in the five common buckets (string)
Employment Status - current employment designation for the customer applying for the loan (string)
Employment Sector - categorical representation of the industry the customer works in (string)
Monthly Gross Income - the amount of money the user makes pre-taxes and deductions (int)
Monthly Housing Payment - how much a month do they pay in housing costs (int)
Ever Bankrupt or Foreclose - has the customer ever had to file for bankruptcy or had a foreclosure on their house (bool)
Loaner - the lending partner (string)
Approved - did the customer get approved for the loan or not (bool)
Bounty - how much did we get paid for the application - only occurs on an approval (int)


# 
