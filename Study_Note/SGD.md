# SGD  
Instead of linear and logistic regression  
  
![...to get this](Attachments/72179E9F-CEBC-4311-AEBD-E3CCF08B6015.jpeg)  
![Sum of squared residuals = (Observed Height - (intercept + slope × Weight))2](Attachments/6166E23F-5772-4B23-8C51-BAA15EDA0FE9.jpeg)  
![a intercepa](Attachments/1AE65B13-54F8-4F15-9CB6-EC57C75B8EF0.jpeg)  
And repeat the derivative until the steps become small  
Let’s suppose we have to calculate 1,000,000 terms for 23,000 derivatives we have to calculate 23,000,000,000 terms. And we need at least 1,000 steps. We need many steps.  
So SGD reduce the computing terms by factor of 3.  
Good to use where there are redundancy (စုဖွဲ့မှု) in dataset. Choose one data and do the math same as Gradient Descent.   
Changing learning rate is called sechdule.   
![Sum of squared residuals =](Attachments/83A8C20F-F1BB-4F85-9C0F-A768BD4EEFF2.jpeg)  
Do the math and we got new intercept and new slope then we pick another data from different redundancy and do the math again. Repeat again and again with new data.  
We can also use 3 data per step which is called mini-batch. Much faster.  
The advantage of SGD is if there is new data, we don’t have to start from the scratch, we can simply calculate the new slope and intercept.  
