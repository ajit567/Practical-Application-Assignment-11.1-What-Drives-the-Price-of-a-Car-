# Practical-Application-Assignment-11.1-What-Drives-the-Price-of-a-Car-
The purpose of this analysis is to provide recommendations to a client -- a used car dealership -- as to what consumers value in a used car and to assist in better inventory management
<h1>
**Practical Application Assignment: What drives the price of a car? **</h1>

The provided dataset contains information on 426K used cars and the goal is to provide clear recommendations to your client -- a used car dealership -- <br />
as to what consumers value in a used car.<br />
The data set contained 18 columns of which only 4 were numerical (id, price, year and odometer). The data set also had columns with lots of null values.<br />
<p>&nbsp;</p>
Following methodology was used for analyzing the data: <br />
1) Slicing the data based on total unit sales per price point bucket and age <br />
The plots were created for slicing the data by car age, odometer miles and price points. Following was observed. <br />
Unit sales wise 97% of the sales were coming from the <50K USD price point cars while revenue wise they only represent 20%. <br />
Majority of the revenue of the >50K USD were coming from just 10 models <br />
I decided to focus on >50K slice of data as it had more data to come up with a prediction on what drives car prices. <br />
The other filtering was to select all cars older than 30 years as only a few cars were in that range (~14k cars age more than 30 years in the data set) and cars usually have <br />
a useful life of around 12 years (30 years is 2x that). <br />
Cars from the year 2022 formed a very low proportion of dataset (133 units) and were also dropped to facilitate creation of a new column called age which measured the <br />
age of car with respect to 2022. <br />
<p>&nbsp;</p>
2) Data preparation: <br />
 4 major activities were done: <br />
 1) Filling the missing data: <br />
 The was done by a four-step process: <br />
 i) checking if the missing data is available elsewhere in the data frame based on the technical specification of the engines for a given model and year <br />
 ii) if available filling it with the above <br />
 iii) filling the rest based on the mode (most frequent) occurring option for that model/year/engine <br />
 iv)checking to see the plot of average price of the important components changed after completion of missing values. As an example - say electric fuel type <br />
has a mean above mean price, it was checked after filling the missing data if this changed for electric or any other fuel type in the category. <br />

 2) creating a region index to account for possible price variation by region. <br />
 A list of top 500 cars that we sold were created for the same options like 6 cylinders, gas fuel automatic transmission etc and the unit price calculated based on sales. <br />
 This unit price was normalized to come up with an index. This better can be better done by replacing with the Used Cars and Trucks in U.S. City Average. <br />

  3) Grouping the number of option provided in each feature to reduce the impact of dimensionality while doing One hot encoding (OHC). There was a total of 112K possible combinations <br />
  of options and these were combined and reduced to 12 to reduce variability in data by aggregation. Details may be found in the Jupiter notebook. <br />
   
  4) Columns with high VIF/multicollinearity were identified and the issue addressed. This was done for use of features with Stats OLS models. <br />
<p>&nbsp;</p>
3) Modelling <br />

A total of 6 models were created. Two were from statsmodel OLS and the rest from Sklearn. A summary is provided: <br />
1) First model: OLS first model was created with scaling and dummy variables to quickly see the results. The model however was seen predicting negative prices and hence <br />
log transformation of the target variable was done. A constant was not put as that might serve as an initial value which I did not want and the coefficients predicted were statistically <br />
significant. <br />
2)Second model: OLS second model was created with log transformation of target variables all the coefficients predicted were statistically significant as well (p value<0.05) <br />
3) Sklearn model:  Ridge regression with OHC, small regularization and no Scaling. This model was made using the transformed target regressor with logarithmic transformation of the <br />
the target variable. This model gave insights into different coefficients without scaling as well and did not show any coefficients with high variability. <br />
4)SKC Ridge with OHC, small regularization and Scaling <br />
5) Ridge with OHC with scaling and regularization after cross validation <br />
6) Lasso with OHC, scaling and regularization after Cross validation- to see if it reduces the number of coefficients in the model <br />

A general comment: <br />
1) Model accuracy: The model had an r2 of 0.55. As we are mainly interested in understanding the relationships between the variables, R-squared values does ### not negate the importance <br />
 of any significant variables. Even with a low R-squared with statistically significant P-values will identify relationships and coefficients will have the same interpretation.  <br />
There is no additional cause to discount these findings.  <br />
2) Coefficients: With log transformation the coefficients used in the model have the following interpretation. Therefore <br />
100 × ([10^𝛽1] − 1) is the percent change in 𝑦 associated with a one-unit increase in the given X when all other X's are held constant. This is needs to interpret for model without scaling and log <br />
i.e model 3 above which has no scaling. Example in model 3 age has a coefficient of -0.000001 while cylinders_category_cylinders_6_8_10_12 a coefficient of 0.124391.  <br />
This would mean -0.000230258 % change in price of car per odometer units or ~ 23% reduction in median for 100K units of odometer while there is a 33% increase in median price if the cylinder happens to be <br />
6,8,10 or 12.  <br />

3) Above evaluation metric: Median absolute error was chosen due to 
a) for log transformations median makes sense
b) MAD is resistant to outliers.

4) Models were created using SKlearn guidelines. 

<p>&nbsp;</p>

4) Inventory tuning:
There are around 26 models in the dataset. This model recommends keeping an eye out on the inventory of:
a) 10 models for the >50K price point cars as they constitute 90% of sales
b) ~1800 models on the <50k price point category as they constitute 90% of the sales
