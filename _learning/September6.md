---
title: "September 6, 2024"
collection: learning
type: "journaling"
permalink: /teaching/September6
venue: ""
date: 2024-09-06
location: ""
---

Daily learning/experience notes for tracking and comprehension checks.

AM Session
======

Today was a continuation of exploring and implementing the unit testing of the backend functions to handle univariate data anomaly detection. The text required creating a specific testing function to handle one aspect of the desired output, specifically dataframe size. The function was set up in a concise manner, split into 3 distinct sections arrange (set up test parameters), action (execute the function with test parameters), and assert (assess if the function executed as expect). We are able to use the `@pytest.mark.parametrize` decorator to easily set up additional inputs to test execution with varying types of data. In the end we ending up with: 

<code>
from src.app.models.univariate import *  
import pandas as pd  
import pytest

@pytest.mark.parametrize("df_input", [
    [1,2,3,4,5,6,7,8,9,10], 
    [1], 
    [1,2,3,4,5.6,7.8,9,10], 
    []
])
def test_detect_univariate_statisticals_returns_correct_number_of_rows(df_input):
    # Arrange
    df = pd.DataFrame(df_input, columns=["value"])
    sensivity_score = 75
    max_fraction_anomalies = 0.20
    # Act
    df_out, weights, details = detect_univariate_statistical(df, sensivity_score, max_fraction_anomalies)
    # Assert
    assert(len(df) == len(df_out))
</code>


The book also briefly hit on how to use postman for performing integration tests between the API and the backend functions. Which provided a good starting point for how I can introduce these concepts into my development practices moving forward.


Bonus learning for me today, the importance of the `__init__.py` file. This is something I've never had to be concerned with my day to day Jupyter coding and the scripts I do write outside of Jupyter seldom references modules outside of the current working directory. So when I learned my initial execution of the test code wasn't workng and traced it to the missing `__init__.py` file, essentially directories without that file's presence are skipped when loading local modules. You can learn more with this informative [Stackover](https://stackoverflow.com/questions/448271/what-is-init-py-for) article.


Looking ahead
======

Tomorrow is Saturday, we learn on everyday that ends with a Y, so the journey continues. The next chapter focuses on implementing the actual code for the anomaly detection functions, looking forward to learning that piece and hopefully find ways to apply it to my threat hunting work.

