---
layout: post
title: "Impact of factors on loans' outcome"
subtitle: "Analyzing correlation between factors and interest rate and loans' final outcome"
background: '\img\posts\loans\jonathan-cooper-0O2Pp6-mOkY-unsplash.jpg'
---
Photo by <a href="https://unsplash.com/@theshuttervision?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Jonathan Cooper</a> on <a href="https://unsplash.com/s/photos/banking?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
  



# Effects of different factors on loans' interest rate and outcome (paid back or not)
## by Estitxu Larralde Erasun


## Investigation Overview

I'd like to look into the correlation of different variables as loan original amount and the interest rate of the loan and the final outcome: was the loan paid back or not (loan status)?

I'm also interested by the correlation of these two variables among them: loan status and borrower APR (annual interest rate) as well as their correlation with the employment status of the borrowers.



## Dataset Overview

The dataset included data on 114 000 small loans from the peer-to-peer lending company Prosper. There were 81 variables on each loan.

After cleaning the dataset of duplicates, null values and irrelevant rows and simplifiying some categorical data, the dataset had around 48000 entries, with 11 variables for each of them.



## Loan Status distribution

Most of the loans in the sample are succesful (ie have been paid back to the lender(s)). Nevertheless, the quantity of unsuccesful loans is very significant: around 28% of the loans.


    
![png](\img\posts\loans\output_15_0.png)
    


## Borrower APR distribution

The annual percentage rate of the borrowers or annual interest rate's histogram shows a multimodal distribution, with interest rates between 0.6% and 42%, with a high peak at 35.5-36% of interest rate.

    
![png](\img\posts\loans\output_17_0.png)
    


## Loan Status vs. BorrowerAPR

As it can be observed in the violinplot below, the BorrowerAPR is in average lower for Successful loans. We can also observe a concentration of high interest rates on the upper side of the figure of unsuccesful loans, more precisely around 30% and 36% of interest rate.
Succesful loans' interest rate concentrates around 12%.

    
![png](\img\posts\loans\output_19_0.png)
    


## Loan Status vs. Employment Status

As we can see below there is a clear correlation between the employment status and the loan status: borrowers whose Employment Status was Employed, 70% successfuly paid the loan, 30% didn't.  
As the employment status of the borrowers worsens, the proportion of succesful loans does, too. 

    
![png](\img\posts\loans\output_21_0.png)
    


## Borrower APR vs. Employment Status

The average interest rate for Not Employed and Other categories of Employment Status are higher than those of the Retired and Employed borrowers. Besides, we can observe a high concentration of the highest interest rate on the top of the violin figure that represents the group Other and to a lesser extent on the figure for Not Employed borrowers.


 
![png](\img\posts\loans\output_23_0.png)
    


## BorrowerAPR vs. EmploymentStatus vs. LoanStatus

Besides confirming the correlation between these three variables, we can see that succesful loans for the unemployed and 'other' employment status borrowers got a similar average interest rate than unsuccesful loans of employed and retired borrowers. In other words, the employment status of borrowers seem to factor quite heavily in the interest rate granted for the loans.

    
![png](\img\posts\loans\output_15_0.png)
    


Ps: To see the code behind the post, check <a href="https://github.com/Pitxunet/Effects-factors-on-loans-rate-outcome/blob/main/Part_II_Prosper_loan.ipynb">this</a> Jupyter Notebook in my GitHub repository!