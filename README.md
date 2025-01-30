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


### Preliminary Intuition
RV Assessment by Kyle Chang
Preliminary intuition
Before getting started with the technical aspects of this case assessment, I thought it would be useful to go through the intuition on the first question, specifically on which of the variables would or would not be helpful in predicting loan request approval.

To me, the Employment Sector variable would have been helpful in the sense that it's an indicator for income, which directly impacts if a customer can pay off a loan in a timely manner. However, because monthly gross income was already available in the dataset, I found the Employment Sector to be redundant and possibly too correlated with income to be used in statistical models. As a side note, though, if there was additional data on job stability within each Employment Sector, I believe it would be useful information, so I chose to include this in the "Potential Next Steps" portion of this report.

Otherwise, the other variables seemed important in the approval process, so I began with exploring statistical tests to back my intuition up.

### Preprocessing
<img width="1665" alt="image" src="https://github.com/user-attachments/assets/318abe8f-0c5c-4045-a711-bc87a24e9e07" />
<img width="1666" alt="image" src="https://github.com/user-attachments/assets/e634d9ba-28eb-40c1-a3d7-59cc296974b5" />
Here, I found about 6500 missing values in the Employment Sector column. From a look at the dataset, these NaNs seem to only appear for unemployed customers.

The following code, however, shows one customer working full time but with a missing value in their Employment Sector column. Since this column was yet to be considered as a significant variable, I decided to wait until the statistical tests were conducting rather than handle it now.
<img width="1670" alt="image" src="https://github.com/user-attachments/assets/a69fef54-eed1-4bc3-b30f-ea7f0fd87d08" />

### Statistical testing for significance
To test significance of the variables, I chose to use t-tests and Chi-square tests.
<img width="1679" alt="image" src="https://github.com/user-attachments/assets/234484e0-1463-4a9e-b866-f36a899e7a83" />
Above, the t-tests resulted in all continuous variables being statistically significant. This makes sense as FICO score, income, payments, and loan amount are all directly related to the ability for a customer to pay off a loan in a timely manner, which is a main factor of whether or not a customer is approved for a loan.
<img width="1672" alt="image" src="https://github.com/user-attachments/assets/0dcf698a-f0b8-4d0d-b9e7-0deb20271569" />
In the same vein, the Chi-square tests showed that FICO score group and employment status were significant, as well as the Ever Bankrupt or Foreclose boolean column (which makes sense because if a customer has a history of being bankrupt or needing to foreclose their home, it's likely they are not equipped to pay off loans).

However, the tests resulted in the Reason for loan as NOT a statistically significant variable, which means the reason for a loan doesn't contribute much to whether a loan request is approved or not. This is a bit shocking to me, as I believe the reason for a loan request could sway how likely a loan is approved (especially if it's for a more serious reason).

It's also important to note that Employment Sector was actually significant, contrary to what I believed to be the case. But we will look into this a bit more to see if anything else is at play behind the scenes.
<img width="1670" alt="image" src="https://github.com/user-attachments/assets/4ca43aba-804d-4676-adee-903b85c139db" />
The above code was written to view how the approval rates may vary between the groups of each categorical variable. Hypothetically, if the approval rate varies greatly, then that should indicate a relationship between that variable and loan approvability. For example, the FICO score group varied greatly, with the "excellent" group having an approval rate of close to 50% and the "poor" group having a rate of less thatn 3%. On the contrary, the approval rates for Reason hovered closely around 11% regardless of the reason, which is consistent wiht the Chi-squared test.
<img width="1672" alt="image" src="https://github.com/user-attachments/assets/86a8d667-cda0-4f08-ba2c-26a7a8e81e70" />
While the Employment Sector column was found to be statistically significant, I suspected that there was a reason behind it, as the result went against my original intuition. To explore this, I graphed the loan approval rates by Employment Sector in a bar chart, while including a line graph of montly income vs. Employment Sector on top of the bar chart.

This showcased a clear correlation between Employment Sector and Monthly Income, which followed a very similar trend to the one found between Employment Sector and Approval Rates. This is consistent with the idea that some industries have higher incomes than others and therefore different approval rates. So, there is some redundancy in including both Employment Sector and Monthly Income, as they portray the same information.

Furthermore, I ordered the Employment Sectors by approval rate and presented both the mean monthly income of each sector as well as the full time rate. I defined the full time rate to be the percentage of jobs within each sector that were full time. As shown in the table, some industries have proportionally more full time jobs than others, greatly affecting monthly income. Similar to the graph above, the full time rate followed the same trend as the approval rate when measured by Employment Sector.
<img width="1661" alt="image" src="https://github.com/user-attachments/assets/6a4c18f6-56c3-424d-a9bc-7222daaf6aa6" />

At this point, I got rid of the Employment Sector variable (for its redundancy) and the Reason variable (for its statistical insignificance). I then chose to ignore the FICO Score Group and continued with just the FICO Score, as the continuous version contains more information and would therefore have more predictive power.

I also chose to combine the Monthly Gross Income and Monthly Housing Payment into a Net Income column (accomplished by subtracting the latter from the former). I felt that this particular combination simplified the data without losing much information from either of the two variables. I see this column as a concise way of representing how much money the customer can use to pay off loans. This comes in handy in the case when two customers have similar income but vastly different housing payments, which can lead to one customer getting a loan approved while the other doesn't. When running the t-test on this new variable, it was still statistically significant with a p-value of 0.

<img width="377" alt="image" src="https://github.com/user-attachments/assets/e23b4103-63a2-4bae-a1eb-1617c46ec99c" />

### How Lenders differ from each other
For this portion, I checked to see how the approval rates differed by Lender, and to no surprise the approval rate seemed to be related to how much revenue per approval each lender offered the company (the higher the revenue, the lower the approval rate).
<img width="1677" alt="image" src="https://github.com/user-attachments/assets/309ef4ba-8d0c-4e4b-972b-82f37e8c861e" />
<img width="623" alt="image" src="https://github.com/user-attachments/assets/8fc50f93-9d1a-462b-8fe0-5522c2fc15a1" />
To explore how each lender is different, I aggregated the data by lending parter and displayed the mean and standard deviations for the numerical columns. I chose to only consider borrowers that were approved loans, since that would be more representative of the requirements of each loaner.

Looking at the table above, one thing that I noticed was that the loan amount did not change much between each loaner. This suggest that loan amount doesn't play as much of a role in approvability as do the other numerical variables. These other variables showed a clear pattern, with loaner B requiring a much higher FICO Score and Net Income than loaner C. This is possibly due to the fact that all three loaners have the same loan offer, meaning that the actual loan amounts should generally be the same.

Another thing that I noticed was that for loaner B, the mean and standard deviation of the Ever Bankrupt or Foreclose column were both 0. This meant that in the 100,000 customers in the dataset, every one that was bankrupt or foreclosed their home were essentially barred from getting their request approved by loaner B.

### My Proposed Loan Matching System
Given what I now know of (a) which variables are most significant in predicting approvability of loan requests and (b) how each lender differes from another, I came up with a simple yet robust solution to finding an improved matching system that maximizes both company revenue and customer approval rate.

First, the system begins by aggregating the dataset by loaner and training a logistic regression model using the Loan Amount, FICO Score, Net Income, Employment Status, and Ever Bankrupt or Foreclose variables as predictors. During this step, it is essential to use a train-test split to avoid overfitting these models.

There should now be one model per loaner, based on historical data of customers that applied to the corresponding loaner. Then, for each new customer, run each model and return the predicted probability (the calculated probability that a customer will be approved by the corresponding loaner). After this is done, multiply each probability with the revenue per approval amount that was set by the probability's corresponder loaner.

The resulting values are the "Expected Revenues," which is the amount that the company could expect to receive from matching a customer to a specific loaner. Now, the system compares these expected revenues and matches the new customer with the loaner with which the expected revenue is the highest.

This system works due to the law of large numbers, where when applied on a large scale of customers and with well trained models, the optimal revenue is sure to be obtained, maximizing the company's revenue and their customers' approval rates.

### Testing the system
The below code will train models on randomly chosen training data, aggregated by loaner. Then the code is run on testing data, which is an accumulation of the testing data sets from each loaner group. This ensures that no training data is leaked into the testing data, causing an overfit model. The expected revenues that were chosen are then summed up and compared with the total revenue of the current system (original data). Similarly, the probabilities that were chosen are averaged out and compared against the approval rate of the original data.
<img width="1670" alt="image" src="https://github.com/user-attachments/assets/0e4ab777-2c29-42ca-b28d-7082f0554c7f" />
<img width="790" alt="image" src="https://github.com/user-attachments/assets/487b3635-4496-4557-b9b1-65315ba69e9e" />
As shown above, the proposed matching system led to a +33.4% increase in total revenue and a +63.94% increase in approval rate. This is a significant improvement from the current system. To explore the relationships between the chosen predictors and approvability, below is a look into the coefficients for each model

<img width="1547" alt="image" src="https://github.com/user-attachments/assets/fefe06f2-5eb0-4ef5-aad8-400a4ee61c24" />
In the graph above, we have each predictor on the x-axis, each with three bars measuring their coefficient values in each of the three models.

For FICO Score and Ever Bankrupt or Foreclose, the coefficients are strongest for Loaner B then A then C (in order of the revenue per approval amounts). This suggests that for stricter loans (loaner B with the lowest approval rate), these variables matter the most for approval, whether positively or negatively.

The other variables are not nearly as strong as the previously mentioned two, but in general the coefficients for loaner B are greater than that of loaner C, suggesting similar things as with FICO and Bankruptcy.

An interesting couple things to note are how for Employment Status, loaner A seems to experience the opposite effect as the other two loaners. For example, while probability for getting approved from loaner B or C are lessened when you are unemployed, it is actually slightly improved for loaner A. Meanwhile, this effect is observed in the opposite effect for part timers, where part time customers are penalized when applying for a loan for lender A. One interpretation is that loaner A is targeted for unemployed customers. But this is contradicted by the fact that Net income and FICO score are positively influential for getting approved for loan A. So, it's possible that loaner A's approval process is more complex than what a logistic regression could capture.

### Conclusion and potential next steps
As demonstrated by the potential increase in revenue and approval rate, this proposed matching system can dramatically improve the company's ability to correctly match their customers with the appropriate loaner, as well as to maximize revenue based on historical data that is easily capturable.

This system could also possibly be further optimized with a few tweaks: 1. Experimentation with more complex machine learning models. As mentioned before, it's possible that loaner A's approval process is more complicated than what a logistic regression can capture. In this case, a more complex model may be better suited, but with the cost of more computational power, which can decrease overall revenue for the company. 2. If data exists on job stability between job industries, this data could be merged with the current dataset and used as a significant predictor of approvability. This is due to the fact that loans are paid in the long term, so stability of income is important in determining whether or not a customer can pay off their loan. 3. A hard filter could be introduced before running the models to avoid expensive computations, especially when experimenting with more complex machine learning models. For example, if a customer was ever bankrupt or foreclosed their home, they wouldn't be considered for loaner B.

With these steps, the proposed system could become even better than now, driving more revenue increase and approval rate improvement, making the company all the more profitable and reliable.

