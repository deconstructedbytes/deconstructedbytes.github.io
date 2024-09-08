---
title: "September 8, 2024"
collection: learning
type: "journaling"
permalink: /learning/September8
venue: ""
date: 2024-09-08
location: ""
---

Daily learning/experience notes for tracking and comprehension checks.


Reading and applying...
==
Finding Ghosts in Your Data: Anomaly Detection Techniques with Examples in Python
==================


Good finish to the chapter, start of the week/end of the week depending on perspective I suppose. Following the text we created a dictionary of weights for each individual test and a function to calculate the "anomaly score" for each variable based on the results of each test. 

Weighing is ultimately the programmers decision, it depends on confidence of the tests and making an informed decision with domain knowledge. For the purpose of the this first installment the author made the determination for the weights to be  - "sds" = 0.25, "iqrs" =  0.35, "mads" = 0.45.

After completing the scoring functions we added some test codes, which actually found some errors that I mistakenly introduced. I'm quickly seeing the value of using pytest during development. It's a lot different from my normal workflow, but I can see how code created this way is more robust and resilient to bad coding practices.


Added a notebook to the repo to play around with the univariate anomaly detection functions some more, [Jupyter](https://github.com/deconstructedbytes/Notebooks/blob/main/AnomalyDetection/UnivariateAnomalyDetection_1.ipynb). Including some zeek connection logs for more real-world application.

#### Code Added

```
def score_results(
        dataframe,
        weights
):
    """
    Take a dataframe and dictionary of weights
    """
    return dataframe.assign(anomaly_score=(
        dataframe["sds"] * weights["sds"] + 
        dataframe["iqrs"] * weights["iqrs"] +
        dataframe["mads"] * weights["mads"]
    ))

def determine_outliers(
        dataframe,
        sensitivity_score,
        max_fractional_anomalies
):
    sensitivity_score = (100 -  sensitivity_score) / 100.0
    max_fractional_anomaly_score = np.quantile(dataframe.anomaly_score,
                                           1.0 - max_fractional_anomalies)
    if max_fractional_anomaly_score > sensitivity_score and max_fractional_anomalies < 1.0:
        sensitivity_score = max_fractional_anomaly_score
        
    return dataframe.assign(
        is_anomaly=(dataframe.anomaly_score > sensitivity_score)
        )
    

def detect_univariate_statistical(
        dataframe,
        sensitivity_score,
        max_fractional_anomalies
):
    weights = {
        "sds": 0.25,
        "iqrs": 0.35,
        "mads": 0.45
    }
    #print(dataframe)
    if (dataframe.value.count() < 3):
        return (dataframe.assign(is_anomaly=False, anomaly_score=0.0), weights, "Must have minimum of 3 items for anomaly detection")
    elif (max_fractional_anomalies <= 0.0 or max_fractional_anomalies > 1.0):
        return (dataframe.assign(is_anomaly=False, anomaly_score=0.0), weights, "Must have valid max fraction of anomalies, 0 < x <= 1.0")
    elif (sensitivity_score <= 0 or sensitivity_score > 100):
        return (dataframe.assign(is_anomaly=False, anomaly_score=0.0), weights, "Must have valid sensitivity score, 0 < x <= 100.0")
    else:
        df_test, calculations = run_tests(dataframe)
        df_scored = score_results(df_test, weights)
        df_out = determine_outliers(df_scored, sensitivity_score, max_fractional_anomalies)
        return  (df_out, weights, {"message" : "Ensemble of [mean +/- 3*SD, median +/- 3*MAD, median +/- 1.5*IQR],",
                                "calculations": calculations}) 
```

#### Unit Test Functions Added today

```
anomalous_sample = [1, 1, 2, 2, 3, 3, 4, 4, 5, 5, 6, 7, 8, 9, 10, 2550, 9000]
@pytest.mark.parametrize("df_input, sensitivity_score, number_of_anomalies", [
    (anomalous_sample, 100, 17),
    (anomalous_sample, 95, 15),
    (anomalous_sample, 85, 8),
    (anomalous_sample, 75, 5),
    (anomalous_sample, 50, 2),
    (anomalous_sample, 25, 2),
    (anomalous_sample, 1, 1)
])
def test_detect_univariate_statistical_sensitivity_affects_anomaly_count(df_input, sensitivity_score, number_of_anomalies):
    # Arrange
    df = pd.DataFrame(df_input, columns=["value"])
    max_fraction_anomalies = 1.0
    # Act
    (df_out, weights, details) = detect_univariate_statistical(df, sensitivity_score, max_fraction_anomalies)
    num_anomalies = df_out[df_out['is_anomaly'] == True].shape[0]
    # Assert:  we have the correct number of anomalies
    assert(num_anomalies == number_of_anomalies)

@pytest.mark.parametrize("df_input, max_fraction_anomalies, number_of_anomalies", [
    (anomalous_sample, 0.0, 0),
    (anomalous_sample, 0.01, 1),
    (anomalous_sample, 0.1, 2),
    (anomalous_sample, 0.2, 3), 
    (anomalous_sample, 0.3, 5), 
    (anomalous_sample, 0.4, 6),
    (anomalous_sample, 0.5, 8), 
    (anomalous_sample, 0.6, 9), 
    (anomalous_sample, 0.7, 12),
    (anomalous_sample, 0.8, 12),
    (anomalous_sample, 0.9, 15),
    (anomalous_sample, 1.0, 17)
])
def test_detect_univariate_statistical_max_fraction_anomalies_affects_anomaly_count(df_input, max_fraction_anomalies, number_of_anomalies):
    # Arrange
    df = pd.DataFrame(df_input, columns=["value"])
    sensitivity_score = 100.0
    # Act
    (df_out, weights, details) = detect_univariate_statistical(df, sensitivity_score, max_fraction_anomalies)
    num_anomalies = df_out[df_out['is_anomaly'] == True].shape[0]
    # Assert:  we have the correct number of anomalies
    assert(num_anomalies == number_of_anomalies)
```


Looking ahead
======

Previewing the next chapter looks like it'll get into more ensembling techniques, I'll probably continue to poke around with the code in Jupyter with some connection logs I have from malware infections to see if anything pops out, also see how manipulating the sensitivity score changes the output.

