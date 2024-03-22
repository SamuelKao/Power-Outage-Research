# Power Outage Research
## Introduction

We are planning to conduct analysis on the outages dataset. It seems the most relevant and has real world applications; thus, we believe the power outages dataset to be the most appealing.

There are various questions that we can explore when it comes to this dataset. Here are a few of them.

- Which factors most heavily contribute to increasing the duration of a power outage?

- Do different regions/states have different distributions in the number of power outages that they experience?

- Has there been an increase in the number of power outages across the United States due to the increase of rapid urbanization?

## Data Cleaning and Exploratory Data Analysis
First, we check all the columns and drop the one we do not need. We are ultimately trying to predict which variables are going to be the most effective at determining how many customers are affected by outage. 

### Univariate Analyses 
The right-skewed shape of the outage duration histogram indicates that there are fewer occurrences of longer outage durations compared to shorter ones. This suggests that the majority of outages tend to be shorter in duration, with only a small proportion of outages lasting for an extended period.
<iframe src="assets\fig1.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets\Distribution-Customers_Affected.html" width=800 height=600 frameBorder=0></iframe>



### Bivariate Analyses
Let's create a box plot to compare the distribution of outage durations ('OUTAGE_DURATION') across different climate regions ('CLIMATE.REGION'). This will help us identify if there are any differences in outage durations based on climate regions.
<iframe src="assets\boxplot.html" width=800 height=600 frameBorder=0></iframe>

We create a scatter plot to visualize the relationship between 'OUTAGE_DURATION' and 'CUSTOMERS.AFFECTED'. This will help us understand if there's any correlation between the duration of outages and the number of customers affected.
<iframe src="assets\Outage_customers.html" width=800 height=600 frameBorder=0></iframe>


### Interesting Aggregates
The graph displays the number of outages per year, with each point representing a specific year. The horizontal axis represents the years, while the vertical axis indicates the count of outages. The plot allows for the visualization of trends or patterns in outage frequency over time, helping to identify any fluctuations or significant changes in outage occurrences across different years.

|   YEAR  |  MONTH  |
|--------:|:-------:|
|  2000   |    19   |
|  2001   |    15   |
|  2002   |    16   |
|  2003   |    46   |
|  2004   |    71   |
|  2005   |    55   |
|  2006   |    66   |
|  2007   |    56   |
|  2008   |   111   |
|  2009   |    78   |
|  2010   |   106   |
|  2011   |   269   |
|  2012   |   174   |
|  2013   |   153   |
|  2014   |   112   |
|  2015   |   119   |
|  2016   |    59   |


<iframe src="assets\scatter_plot.html" width=800 height=600 frameBorder=0></iframe>
## Assessment of Missingness
### NMAR Analysis


### Missingness Dependency

#### 1. CUSTOMERS.AFFECTED and POSTAL.CODE (MCAR)
Null Hypothesis: The missingness of Customers Affected does not depend on Postal Code

Alternative Hypothesis: The missingness of Customers Affected depend on Postal Code

We're analyzing missing values in the 'CUSTOMERS.AFFECTED' column of the dataset. We've created an indicator showing missing (1) and non-missing (0) values. 
Then, we're calculating Total Variation Distance (TVD) for these proportions. This helps measure the difference in distribution between missing and non-missing values across postal codes.

Below is the graph of our result of permutation test, our p-value is greater than our significance level of 5%, so we fail to reject the null hypothesis. The significance of this is that it is highly possible that the missingness of Customer Affected does not depend on Postal Code.
<iframe src="assets\Dist_Perm_Diff.html" width=800 height=600 frameBorder=0></iframe>

#### 2. CUSTOMERS.AFFECTED and OUTAGE_DURATION (MAR)
Null Hypothesis: The missingness of CUSTOMERS.AFFECTED does not depend on OUTAGE_DURATION

Alternative Hypothesis: The missingness of CUSTOMERS.AFFECTED depend on OUTAGE_DURATION

Then, we're calculating Absolute Difference in mean for these proportions. This helps measure the difference in distribution between missing and non-missing values across Outage Duration.

Below is the graph and the output of permutation test. Our p-value is less than our significance level of 5%, so we reject the null hypothesis. The significance of this is that it is highly possible that the missingness of Customer Affected depends on outage duration.

<iframe src="assets\permuted_ks_stats_distribution_with_p_value.html" width=800 height=600 frameBorder=0></iframe>

## Hypothesis Testing
### DEFINING THE HYPOTHESIS

Null Hypothesis: The mean duration of power outages in areas with a RES.PRICE > 11.5 are the same as the mean duration of power outages in areas witha. RES.pRICE <= 11.5. 

Alternative Hypothesis: The mean duration of power outages in areas with a RES.PRICE > 11.5 are different than the mean duration of power outages in areas witha. RES.pRICE <= 11.5. 

Let's visualize the distribution of anomaly.levels in places with a res.price <= 11.5 and places with a res.price > 11.5

<iframe src="assets\Anomaly.html" width=800 height=600 frameBorder=0></iframe>

From this distribution above, its difficulat to tell whether or not there is a difference between the two groups.

### Test Statistic Calculations

Before we begin our permuation test, let's see what our observed statistic is. For the sake of our hypothesis testing, we will be using the difference in grouped means.

Our observed difference in grouped means is 0.01786

### Permutation Test

Now that we've calculated our observed statistic. Let' begin our comparison of these two samples by doing a permutation test. In order to do this test, we are goingt o be randomly shuffling around the values of ANOMALY.LEVEL. We are not shuffling around the RES.PRICE groups so that we can still groupby the "RES.PRICE" later on in our loop.

Once again, the question that we are trying to answer is that how likely is it that one random shuffle gives us a difference in grouped means (RES.PRICE <= 11.5 & RES.PRICE > 11.5) of 0.01786?

### Conclusion

After permutating and shuffling the groups 500 times. We have our observed statistic as well as our simulated difference. By plotting a histogram of the simulated differences against the actauly observed statistic we can answer our question from before.

<iframe src="assets\Mean_Anomaly.html" width=800 height=600 frameBorder=0></iframe>


Under the null hypothesis, we can regularly see a difference like 0.01786. Therefore we fail to reject the null hypothesis that these two groups come the same distribution. The difference between the two samples is not statistically significant. 

## Framing a Prediction Problem

Notebook: We want to predict the duration of a power outages based on the following features: “RES.PRICE”, “IND.PRICE”, “CLIMATE.REGION”, “CLIMATE.CATEGORY”, “COM.PRICE”.

|RES.PRICE	|CLIMATE.REGION	|COM.PRICE|	OUTAGE_DURATION	|PI.UTIL.OFUSA |CUSTOMERS.AFFECTED|
|---------------:|:------------|:--------------------|-------:|
|11.6	|East North Central|	9.18|	3060.0|	2.2|	70000.0|
|12.12	|East North Central|    9.71|	1.0|2.2	|0.0|
|10.87	|East North Central|	8.19|	3000.0|	2.1|70000.0|
|11.79	|East North Central|	9.25|	2550.0|	2.2| 68200.0|
|13.07	|East North Central|	10.16|	1740.0|	2.2|	250000.0|

These are the features that we are going to be using because these are the only features that we can be sure of before the power outage itself. Other factors like “CUSTOMERS.AFFECTED” wouldnt be known to us before the start of the power outage. Thus, these features are going to be the ones we’re training our Regression model on. 

To recap, our response variable is going to be “CUSTOMERS.AFFECTED” and our features that we are going to be using are “RES.PRICE”, “IND.PRICE”, “CLIMATE.REGION”, “CLIMATE.CATEGORY”, “COM.PRICE”, . The metric that we are going to be using to determine the accuracy of our model is the RMSE as well as the R^2. 

