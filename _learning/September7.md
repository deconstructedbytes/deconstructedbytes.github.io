---
title: "September 7, 2024"
collection: learning
type: "journaling"
permalink: /teaching/September7
venue: ""
date: 2024-09-07
location: ""
---

Daily learning/experience notes for tracking and comprehension checks.

AM Session
======

Started reading Chapter 6, Implementing the First Methods and writing code for anomaly detection with univariate datasets. The text reiterated what detecting outliers within a dataset that only contains one variable, basically we're looking to determine averages or commonailites for the data as a whole then identifying values that are further away from these average measurements than most of the other values.

For the tests we defined today for the mid point references we're using are the mean and median of the dataset. To calculate how far away from those values is enough to consider the value an outlier we're using the standard deviation (sd) and the median average deviation (mad), respectively. Any value that is a measurement away from the mid point that is greater than 3 times the sd or mad will be considered outliers.

Additionally, we've created a third test that uses the [interquartile range](https://www.geeksforgeeks.org/interquartile-range-to-detect-outliers-in-data/) or IQR, for this test values that a less than the 25th percentile of the IQR multipled by 1.5 or greater that than the 75th percentile of the IQR multiple by 1.5 will be considered outliers.

The chapter also delved into the concept of ensembling usage in simple statistics tests to add some level of confidence in the test findings. While I've never heard the work ensembling, it sounds like what machine learning forests do for more confident predictions. Individually, the above tests are considered methods to determine if a value is an outlier or not, based purely on the constraints of the test. However, each performs differently with the data and depending on the distribution of the data could fail to produce expected results. For normally distributed data using the mean and standard deviations should identify outliers, however, when distribution is skewed using MAD and IQR methods are more useful. Ensembing is a method to compare results from individual test to add more confidence in the results, e.g. saying x was found to be an outlier in all three test would have more confidence than if it was found to be an outlier only in 1 of three. etc. There are more components, like weight the output of different test and so on, but that's the gist of it. Definitely something to spend more time digging into, I found [this](https://www.analyticsvidhya.com/blog/2018/06/comprehensive-guide-for-ensemble-models/) reference I'll visit at some time.


#### Todays Code

```
import pandas as pd
import numpy as np
from statsmodels import robust

def check_stat(val:float,
               midpoint:float,
               distance:float,
               n:float):
    """
    Check if a given value is within a given range of a 
    midpoint value and a number of increments. If the value is within 
    this range return a percentage else return 1.0 indicating the value 
    is an statistical outlier
    """
    if (abs(val - midpoint) < (n * distance)):
        return abs(val-midpoint) / (n * distance)
    return 1.0

def check_sd(val:float,
             mean:float,
             sd:float,
             min_num_sd:float):
     """
     Check if a given value is a specified number of 
     standard deviations away from the mean
     """
     return check_stat(val, mean, sd, min_num_sd)

def check_mad(val:float,
              median:float,
              mad:float,
              min_num_mad:float):
    """
    Check if a given value is with the range of
    the median absolute value and a specific length or distance
    If the value is within the range return a percentage, else
    return 1.o indicating it is an outlier
    """
    return check_stat(val, median, mad, min_num_mad)

def check_iqr(val:float,
              median:float,
              p25:float,
              p75:float,
              iqr:float,
              min_iqr_diff:float):
    """
    Check if on which side of the median a value exists
    If below the median checks if the value is min_iqr_diff times below the p25 IQR
    if above checks if the value min_iqr_diff times above the p75.
    if the value passes those checks return 1.0 to suggest the value
    is an outlier
    """
    if val < median:
        if val > p25:
             return 0.0
        elif (p25 - val) < (min_iqr_diff * iqr):
             return abs(p25 - val) / (min_iqr_diff * iqr)
        else:
            return 1.0
    else:
        if val < p75:
            return 0.0
        elif (val - p75) < (min_iqr_diff * iqr):
            return abs(val - p75) / (min_iqr_diff * iqr)
        else:
            return 1.0
        

def run_tests(dataframe):
    """
    Pandas dataframe containing univariate data to perform 
    anomaly detection against
    """
    mean = dataframe.value.mean()
    sd = dataframe.value.std(0)
    p25 = np.quantile(dataframe.value, 0.25)
    p75 = np.quantile(dataframe.value, 0.75)
    iqr = p75 - p25
    median = dataframe.value.median()
    mad = robust.mad(dataframe.value)
    calculations = {
        "mean": mean, "sd": sd, "p25": p25,
        "p75": p75, "iqr": iqr, "median": median,
        "mad":mad
    }
    dataframe["sds"] = [check_sd(val, mean, sd, 3.0) for val in dataframe.value]
    dataframe["mads"] = [check_mad(val, median, mad, 3.0) for val in dataframe.value]
    dataframe["iqrs"] = [check_iqr(val, median, p25, p75, iqr, 1.5) for val in dataframe.value]
    
    return dataframe, calculations
```


Looking ahead
======

Football!! Michigan plays Texas today, the first of many tests this season, should be a good one!! Tomorrow I'm thinking I can wrap up the chapter and fully implement the remain code focused of weighing and scoring results.

