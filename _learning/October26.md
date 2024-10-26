---
title: "September 9, 2024"
collection: learning
type: "journaling"
permalink: /learning/September9
venue: ""
date: 2024-09-09
location: ""
---

Daily learning/experience notes for tracking and comprehension checks.


Reading and applying...
==
Finding Ghosts in Your Data: Anomaly Detection Techniques with Examples in Python
==================

Learned about some tests to check whether or not data follows a normal distribution; the Shapiro Wilf test, the D’Agostino’s K-squared test that checks the skewness and kurtosis of the data distribution, and lastly the Anderson-Darling test.

- **Shapiro-Wilk**
>- This test relies on using a Null Hypothesis and p-values to determine whether or not a dataset is normally distributed. 
- **D’Agostino**
>- D'Agostino tests focus on measuring skewness and kurtosis. Skewness can get either negative, positive or none depending on the direction of the tail. Kurtosis is the measurement of how much of the data falls within the area of the tail. 
- **Anderson-Darling**
>- A statistic test similar to Shapiro-Wilk, but allows testing against different significance and critical values.


Checking how normal the dataset is important when using these simple statistics test for outlier detection, because the normalness of the data influences which methods are more reliable to finding outliers. Additionally, there are times when its possible to transform non-normal data to a normal distribution so our tests perform more accurately.

#### Code Added

```
from scipy.stats import shapiro, normaltest, anderson, boxcox
import scikit_posthocs as ph
import math

def check_shapiro(col, alhpa=0.5):
    """
    Statistics based hypothesis test to check for normality using
    a null hypotheis if the p-value is low the hypothesis is rejected
    and the test assumes the data is not normally distributed
    """
    return check_basic_normal_test(col, alpha, "Shapiro-Wilf test", shapiro)

def check_dagostino(col, alpha=0.5):
    """
    Statistics test to determine how much skewness and kurtosis a dataset
    contains. Skewness = measure of tails, longer left indicates negative skew, 
    longer right indicates positive skew. A Dataset can also have 0 skew.
    Kurtosis is the measure of how much of the dataset is inside the skew
    """
    return check_basic_normal_test(col, alpha, "D'Agostino's K^2 test", normaltest)

def check_basic_normal_test(col, alpha, name, f):
    """
    General setup function that takes 
    the dataset, alpha, name and specific normality checking
    function as inputs and standardizes the output
    """
    stat, p = f(col)
    return ( (p > alpha), ( f"{name} test, W = {stat}, p = {p}, alpha = {alpha}. " ) )

def check_anderson(col):
    """
    Normality test similar to Shapiro but it performs at different 
    significance levels
    """
    anderson_normal = True
    return_str = "Anderson-Darling Test. "
    result = anderson(col)
    return_str = return_str += f"Result statistic: {result.statistic}"
    for i in range(len(result.critical_values)):
        sl, cv = result.significance_levl[i], result.critical_values[i]
        if result.statistic < cv:
            return_str = return_str += f"Significance Level {sl}: Critical Value = {cv}, looks normally distributed. "
        else:
            anderson_normal = False
            return_str += f"Significance Level {sl}: Critical Value = {cv}, does NOT look normally distributed. "
    return (anderson_normal, return_str)
```


Looking ahead
======

I find myself getting through the text a bit slower when these topics around the math and statistics behind the scenes come up and I'm ok with that. Better to have a good understanding of why something works than just having something that works, but I can't reproduce independently or explain why it works. 

