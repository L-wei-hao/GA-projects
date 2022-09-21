# Ames Housing: A Model for Price Prediction
---

### Problem Statement

We are WEM general insurance based in Ames, Iowa specializing in home insurance.
The issue here is that customers has feedback to us that our existing form are Tedious and overly complicated.  
This has translated to higher dropout rate and loss of revenue. 
This has caught the management's attention and has noted that our competitors are offering a quicker and more accurate solution to this issue.


The Ames data set has many parameters (80 independent and 1 dependent variable) that can either assist or distort the predictive model. The problem is which features should be used in our predictive model from the information provided from the parameters and predict Sales Price with the lowest root mean square error with the references from Kaggle 

A successful outcome would be deriving an approach in treating the parameters and transforming the object variables into usable variables in the predictive model. From there, we can best determine if Lasso, Ridge or Linear Regression will be the chosen model for this problem.

---

### Data Dictionary
The Ames Housing Dataset is an exceptionally detailed and robust dataset with 81 columns of different features relating to houses which includes 39 numerical value features and 42 categorical features of which are 2 IDs.

A detailed description of each variable in the data set can be found [here](https://www.kaggle.com/competitions/dsi-us-11-project-2-regression-challenge/data).

---

### Analysing Data set 

With the sales Price as the dependent variable y, we did an anlysis on its distribution.

![ydisplot](images/y_distribution.png)  
![yscatter](images/yscatter.png)  

We can see that there is a pronounced curve on the Sales price. The target variable is right skewed. As linear models tends to work better in a normal distribution, we need to transform this variable and make it more normally distributed.

To normalise it, we used the log function.
---
![postlog](images/postlog.png)  

---

### Skewed features

Some features have a very high % of a single value. Such features are usually poor predictors and can be dropped.  
For e.g. Asking a customer if his house has utilities in this day and age is seemingly redundant.
another e.g. would be asking if the customer's house have a pool or not? (we know a huge majority of people don't!)
For the purpose of this project, we dropped features that has more than 70% of a single value (includes Null values)  
Features listed below were dropped:  
![skewed_features](images/skewed_features.png)  


---

### Feature engineering

Some features were combined

|Feature|Data Type|Data Description|
|:--|:--|:--|
|'Bath'|Numerical > 0|sum of Bath & half Baths|
|'Non Liv Area'|Numerical > 0|'Lot Area' - 'Gr Liv Area'|![postlog](images/postlog.png)  

After dropping redundant columns, we are left with 27 features.

---

### Outliers


![correlation](images/correlation.png)    
We determined the outliers against the top 5 features based on Correlation score.  
1) 'Overall Qual',  
2) 'Gr Liv Area',   
3) 'Garage Cars',   
4) 'Year Remod/Add',  
5) 'Bath'

![outlier](images/outliers.png)  

---

### Categorical Data

We then identified that there are 7 nominal and 8 Ordinal data  
![](images/normord.png)  

![](images/catdata.png)  

Ordinal data are assigned integers based on ranking, while nominal data are one hot encoded.

---

### Models
we created 2 models, they are linear regression and Lasso derived from Grid search.

#### Linear regression
MSE of Train is 0.014056343635805614,  
MSE of Test is 0.01597062015040118,  
Percentage diff: -13.61859502153419%,  

The training has a better MSE score ( neare to 0 ) as compared to the train, the difference is more than 13% which shows that the model is over fitting.
![](images/linearmodel.png)  

#### Lasso  
MSE of Train is 0.020309449808168796,  
MSE of Test is 0.019948253199471917,  
Percentage diff: 1.7784657492375768%  
The Lasso model shows a much better generalization as compared to the linear regression model.  
![](images/lassomodel.png)

However from the above plot, we noted that the model seems to be ineffective in predicting prices above the 400,000 mark, with the tendency to under predict the sales price.

---

### Kaggle score

Even though the linear regression model is overfitted, it scores well in Kaggle  
![](images/my_output_lr.png)  

As compared to the Lasso model  
![](images/my_output_GS.png)  


---

### Multicollinearity

![](images/vif.png)  

Setting the Threshold of VIF as 5, we can see that 1 of the feature "GR Liv Area" fails the multi collinearity test.
This could be due to the fact that the larger the Gross living area, we would expect a larger house which might mean that there are more bathrooms, more cars that can fit into the garage, and also correspondingly a larger non living area.  
However, from a property valuation standpoint, this feature is very important as although generally speaking the bigger the lot size the the higher the price, but living area are usually more expensive than non livable area.  
Also for insurance industry, given that livable area are usually indoors where all the assets are located at.It is generally more expensive to the insurer to cover a large area of livable area as compared to non livable area. As such this feature is essential to the business and dropping it is not an option.  
In conclusion, even though there is multi collinearity noted in our model, it is not exceptionally high, also given the importance of the feature, we would have to make do with it.


---

### Conclusion  

Our model for implementation will be the Lasso Model, We have successfully reduced the number of features from the initial 80 down to 27 features more than 66% reduction. The new form will have 27 questions this will greatly improve customer's ease of use and will drastically reduce the drop out rate due to form complexity.


