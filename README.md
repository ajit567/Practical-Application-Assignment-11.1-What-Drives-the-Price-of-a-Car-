# Practical-Application-Assignment-11.1-What-Drives-the-Price-of-a-Car-
The purpose of this analysis is to provide recommendations to a client -- a used car dealership -- as to what consumers value in a used car and to assist in better inventory management
<h1>
**Practical Application Assignment: What drives the price of a car? **</h1>


The data set contained 18 columns of which only 4 were numerical (id, price, year and odometer). The data set also had columns with null values.<br />
<p>&nbsp;</p>
Following methodology was used for analyzing the data: <br />
1) Slicing the data based on total unit sales per price point bucket and age of the car. <br />
Plots were created for slicing the data by cars age, odometer miles and price points. Following was observed. <br />
a)Unit sales wise 97% of the sales were coming from the <50K USD price point cars while revenue wise they only represent 20%. <br />
b)Majority of the revenue of the >50K USD price point category were coming from just 10 models <br />
I decided to focus on <50K slice of data as it had more data to come up with a prediction on what drives car prices. <br />
The other filtering that was applied was to select all cars older than 30 years as only a few cars in the data set were above that age (~14k cars age more than 30 years in the data set) and cars usually have a useful life of around 12 years (30 years is 2x that). <br />
Cars from the year 2022 formed a very low proportion of dataset (133 units) and were also dropped to facilitate creation of a new column called age which measured the 
age of car with respect to 2022. <br />
<p>&nbsp;</p>
2) Data preparation: <br />
 4 major activities were done as a part of data preparation: <br />
 1) Filling the missing data: <br />
 The was done by a four-step process: <br />
 i) checking if the missing data is available elsewhere in the data frame based on the technical specification of the engines for a given model and year <br />
 ii) if available filling the missing using the  above <br />
 iii) filling the rest based on the mode (most frequent) occurring option for that model/year/engine <br />
 iv)checking to see if the plot of average price of the important components changed after completion of missing values. As an example - say electric fuel type <br />
has a mean above overall mean price, it was checked after filling the missing data if this changed overall mean price for electric or any other fuel type within that <br />  category with repsect to the overall mean. <br />

 2) Creating a region index to account for possible price variation by region. <br />
 A list of top 500 cars that we sold was created with the same options like 6 cylinders, gas fuel automatic transmission etc and the unit price calculated based on <br /> sales. These options were slected based on the highest sales for their respective category.<br />
 This unit price was normalized to come up with an index that acts as a proxy for regional price variation. This can definitely be done better by replacing it with the<br /> Used Cars and Trucks in U.S. City Average data from FRED. I could not find a time efficient way to put all of cities/regions in the data together so did not 
 pursue it <br />

  3) Grouping the number of options provided in each feature to reduce the impact of dimensionality while doing One hot encoding (OHC). There were a total of 112K <br /> possible combinations <br />
  of options and these were combined and reduced to 12 to reduce variability in data by aggregation. Details may be found in the Jupiter notebook. <br />
   
  4) Columns with high VIF/multicollinearity were identified and the issue addressed. This was done for use of features with Stats OLS models. <br />
<p>&nbsp;</p>
3) Modelling <br />

A total of 6 models were created. Two were from statsmodel OLS and the rest from Sklearn. A summary is provided: <br />
1) First model: OLS first model was created with scaling and dummy variables to quickly see the results. The model however was seen to be predicting negative prices and hence <br />
log transformation of the target variable was done. A constant was not put as that might serve as an initial fixed cost which was thougth of as inaccurate <br />
as it acts a fixed cost for all cars and we have car sales going very low in the data set. <br />
All the coefficients predicted by the mdoel were statistically significant. (having p_value<0.05)<br />

2)Second model: OLS second model was created with log transformation of target variables all the coefficients predicted were statistically significant as well (p value<0.05). Hence implying usage for predicting factors that impac price of a car <br />

3) Sklearn model:  Ridge regression with OHC, small regularization and no Scaling. This model was made using the transformed target regressor with logarithmic  <br /> transformation (1og to the base 10) of the the target variable. Log 10 was taken due to the large scale of data which "e" would have taken more units to cover. 
This model gave insights into different coefficients without scaling and did not show any coefficients with high variability. Coeffcients follow the expected  <br />  correlating trend (age and odometer mileage come as negatively correlated)<br />

4)SKC Ridge with OHC, small regularization and Scaling.This changed the coefficients of the numerical features - odometer got more significant than age. <br />
The change was due to scaling. <br />

5) Ridge with OHC with scaling and regularization after cross validation. This showed that none of the selected features were susceptible to variability <br />
 introduced by pertubation of data<br />

6) Lasso with OHC, scaling and regularization after Cross validation- to see if it reduces the number of coefficients in the model as Lasso given sparse  <br />
  coefficients. This howvever did not turn out to be the case and all the features previously selected were kept.

A general comments about model: <br />

1) Model accuracy: The model had an r2 of 0.55. As we are mainly interested in understanding the relationships between the variables, R-squared values does does <br />not negate the importance of any significant variables. Even with a low R-squared with statistically significant P-values will identify relationships and <br /> coefficients will have the same interpretation.  <br />
There is no additional cause to discount these findings.  <br />

2) Coefficients: With log transformation and the use of a "Log linear model_ the coefficients used in the model have a different  interpretation. It can be read as <br /> 100 × ([10^𝛽1] − 1) percent change in 𝑦 associated with a one-unit increase in the given X when all other X's in the model are held constant. This however o<br /> limited to be interpreted for model without scaling and using log transformation and OHC whihc is model 3. AS an Example in model 3 age has a coefficient of -0.000001<br />  while cylinders_category_cylinders_6_8_10_12 a coefficient of 0.124391.  <br />

This would mean -0.000230258 % change in price of car per odometer mileage increase or ~ 23% reduction in median for 100K units of odometer while there is a 33% increase in<br /> median price if the cylinder happens to be <br />
6,8,10 or 12.  <br />

3) Above evaluation metric: Median absolute error was chosen due to 
a) for log transformations use of  median as opposed to mean makes sense. Log transform bring mean and median close together
b) MAD is resistant to outliers while mean is not.

4) Models were created using SKlearn guidelines. 

<p>&nbsp;</p>

4) Inventory tuning was done from an objective of keeping track of smallest number of units which have a significant impact in Revenue($ sales)
There were around 26K models in the dataset. This model recommends keeping an eye out on the inventory of:
a) 10 models for the >50K price point cars as they constitute 90% of sales
b) ~1800 models on the <50k price point category as they constitute 90% of the sales for that category
