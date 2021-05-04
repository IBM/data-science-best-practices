# Data Science - Best Practices &middot; [![License](https://img.shields.io/badge/license-CC%20BY%204.0-blue)](./LICENSE.txt)

## Table of Content

- [Chapter 1 - Introduction](./readme.md#chapter-1---introduction)
- [Chapter 2 - Project Team (Design)](./project_team.md#chapter-2---project-team)
- [Chapter 3 - Architecture (Deploy)](./architecture.md#chapter-3---architecture)
- [Chapter 4 - Source Code (Engineer)](./source_code.md#chapter-4---source-code)
- [Chapter 5 - Documentation (Engineer)](./documentation.md#chapter-5---documentation)
- [Chapter 6 - Versioning (Engineer)](./versioning.md#chapter-6---versioning)
- [Chapter 7 - Data Management (Engineer)](./data_management.md#chapter-7---data-management)
- [Chapter 8 - Dependency Management (Engineer)](./dependency_management.md#chapter-8---dependency-management)
- [Chapter 9 - Configuration Management (Engineer)](./configuration_management.md#chapter-9---configuration-management)
- [Chapter 10 - Testing (Engineer)](./testing.md#chapter-10---testing)
- [Chapter 11 - Quality Measurements (Monitor)](./quality_measurements.md#chapter-11---quality-measurements)
- [Chapter 12 - Model Training (Engineer)](./model_training.md#chapter-12---model-training)
- [Chapter 13 - Distribution (Deploy)](./distribution.md#chapter-13---distribution)
- [Chapter 14 - Cloud-Deployment (Deploy)](./cloud_deployment.md#chapter-14---cloud-deployment)
- [Chapter 15 - Edge Deployment (Deploy)](./edge_deployment.md#chapter-15---edge-deployment)
- [Chapter 16 - Monitoring (Monitor)](./monitoring.md#chapter-16---monitoring)
- [Chapter 17 - Automation (Scalability)](./automation.md#chapter-17---automation)
- [Chapter 18 - Scaling (Scalability)](./scaling.md#chapter-18---scaling)
- [Chapter 19 - Sizing (Scalability)](./sizing.md#chapter-19---sizing)
- [Chapter 20 - Security (Engineer)](./security.md#chapter-20---security)
- [Chapter 21 - Usage Recommendations (Scalability)](./recommendation.md#chapter-21---usage-recommendations)
- [License & Contributing](./license.md)

## Chapter 11 - Quality Measurements

### Code Coverage

Code Coverage measures the percentage of source code that is executed and therefore tested by a given set of tests. ([Wikipedia](https://en.wikipedia.org/wiki/Code_coverage)) 100% means every line of source code has been executed during the tests. 0% means none has been executed during tests; this usually happens when you have not ran any tests.
The Python library [`coverage`](https://pypi.org/project/coverage/) can be used to automatically measure the coverage of `unittest` tests. For unit tests you should aim for a very high percentage value (75% or more).
For integration tests or system tests it is sometimes not possible to create a 100% test coverage.
Therefore its code coverage is usually lower than the unit test code coverage.

### Data Quality Checks

Many of the algorithms and techniques we use to model data make assumptions about the data and many others require the data to be in specific formats and to comply with specific rules before we run them. Most importantly perhaps, checking the quality of your data is key because a model once the data is in the format it expects, will run regardless of whether you replaced missing values with the average value for that column or if you removed them. Performing quality checks on your data allows you to determine whether the potential issues you see are naturally ocurring phenomena in the data or if they are the product of ingestion or other code bugs in your pipeline. Below is a list of some of the most important data quality checks you should perform:

  - *Unbalanced Data* an unbalanced feature is a feature for which one value predominates. Unbalanced features can occur naturally, but if a feature always has the same value you may have a data bug.
  - *Uniformly Distributed Data* A uniformly distributed feature is one for which all possible values appear with close to the same frequency. As with unbalanced data, this distribution can occur naturally, but can also be produced by data bugs.
  - *Missing Data* A data bug can also cause incomplete feature values. For example you may expect a feature's value list to always have three elements and discover that sometimes it only has one.
  - *Large Differences in Scale Between Features* If your features vary widely in scale, then the model may have difficulties learning. For example, if some features vary from 0 to 1 and others vary from 0 to 1,000,000,000, you have a big difference in scale. Compare the "max" and "min" columns across features to find widely varying scales. Consider normalizing feature values to reduce these wide variations.
  - *Data Integrity* refers to the accuracy and consistency of data over its lifecycle. Error checking methods and validation procedures are typically relied on to ensure the integrity of data.
  
One tool that we recommend for data quality checks is the use of [pandas profiling](https://github.com/pandas-profiling/pandas-profiling) which generates profile reports from a pandas `DataFrame`. The pandas `df.describe()` function is great but a little basic for serious exploratory data analysis. `pandas_profiling` extends the pandas DataFrame with `df.profile_report()` for quick data analysis.

For each column the following statistics - if relevant for the column type - are presented in an interactive HTML report:

* **Type inference**: detect the [types](#types) of columns in a dataframe.
* **Essentials**: type, unique values, missing values
* **Quantile statistics** like minimum value, Q1, median, Q3, maximum, range, interquartile range
* **Descriptive statistics** like mean, mode, standard deviation, sum, median absolute deviation, coefficient of variation, kurtosis, skewness
* **Most frequent values**
* **Histogram**
* **Correlations** highlighting of highly correlated variables, Spearman, Pearson and Kendall matrices
* **Missing values** matrix, count, heatmap and dendrogram of missing values
* **Text analysis** learn about categories (Uppercase, Space), scripts (Latin, Cyrillic) and blocks (ASCII) of text data.

### Model Quality Measurements

#### Error Metrics

When we talk about predictive models, we are talking either about a regression model (continuous output) or a classification model (nominal or binary output). The evaluation metrics used in each of these models are different.

In classification problems, we use two types of algorithms (dependent on the kind of output it creates):

- Class output: Algorithms like SVM and KNN create a class output. For instance, in a binary classification problem, the outputs will be either 0 or 1. However, today we have algorithms which can convert these class outputs to probability. But these algorithms are not well accepted by the statistics community.
- Probability output: Algorithms like Logistic Regression, Random Forest, Gradient Boosting, Adaboost etc. give probability outputs. Converting probability outputs to class output is just a matter of creating a threshold probability.

In regression problems, we do not have such inconsistencies in output. The output is always continuous in nature and requires no further treatment.

It is important then that we evaluate the output of our models based on the type of output and also on the type of insight that we wish to obtain. We assume that as a data scientist you are familiar with these metrics and will not cover them here, if you would like to refresh your knowledge of commonly used metrics for both classification and regression [check this link](https://www.analyticsvidhya.com/blog/2019/08/11-important-model-evaluation-error-metrics/)

#### Using baselines
When starting on a project, the first priority is learning about what potentially unforeseen challenges will stand in your way. Even if they will not be the final version of your model, baselines allow you to iterate very quickly, while wasting minimal time.

>     Everything should be made as simple as possible, but not simpler -Albert Einstein

Usually there are three levels of performance you can easily estimate without any code and can serve as a first baseline:

- **The trivially attainable performance**, which is a metric you would hope any model could beat. An example of this value could be the accuracy you would get by guessing the most frequent class in a classification task.
- **Human performance**, which is the level at which a human can accomplish this task. Computers are much better than humans at some tasks (like playing Go) and worse at others (like writing poetry). Knowing how good a human is at something can help you set expectations ahead of time for an algorithm, but since the human/computer difference varies widely by field, it may require some literature search to calibrate.
- **The required to deploy performance**, which is the minimal value that would make your model suitable for production from a business and usability standpoint. Usually, this value can be made more attainable by smart design decisions. Google Smart Reply, for example, shows three suggested responses, significantly increasing their chances of showing a useful result.

Then there are some more data driven baselines you can use. Typical baselines include those supported by [scikit-learn's "dummy" estimators](https://scikit-learn.org/stable/modules/model_evaluation.html#model-evaluation):

Classification baselines:

- *stratified*: generates predictions by respecting the training set’s class distribution.
- *most_frequent*: always predicts the most frequent label in the training set.
- *prior*: always predicts the class that maximizes the class prior.
- *uniform*: generates predictions uniformly at random.
- *constant*: always predicts a constant label that is provided by the user.

Regression baselines:

- *median / mean*: always predicts the median of the training set
- *quantile*: always predicts a specified quantile of the training set,provided with the quantile parameter.
- *constant*: always predicts a constant value that is provided by the user.
- *last observation*: use the last observed value as your next prediction.
- *Rolling average*: use a rolling average as your prediction.

#### Calculating error bounds / confidence intervals

A confidence interval is different from a tolerance interval that describes the bounds of data sampled from the distribution. It is also different from a prediction interval that describes the bounds on a single observation. Instead, the confidence interval provides bounds on a population parameter, such as a mean, standard deviation, or similar.

In applied machine learning, we may wish to use confidence intervals in the presentation of the skill of a predictive model. e.g. Given the sample, there is a 95% likelihood that the range x to y covers the true model accuracy.

> Estimates of uncertainty help in two ways. First, the intervals give the consumers of the model an understanding about how good or bad the model may be. In this way, the confidence interval helps gauge the weight of evidence available when comparing models. The second benefit of the confidence intervals is to facilitate trade-offs between models. If the confidence intervals for two models significantly overlap, this is an indication of (statistical) equivalence between the two and might provide a reason to favor the less complex or more interpretable model.

In machine learning applications we often deal with non-parametric models for which often we do not know the distribution for a chosen performance measure. Furthermore, we may not know the analytical way to calculate a confidence interval for a selected metrics like e.g. AUC or RMSLE.

In these cases, the bootstrap resampling method can be used as a nonparametric method for calculating confidence intervals, nominally called bootstrap confidence intervals.The bootstrap is a simulated Monte Carlo method where samples are drawn from a fixed finite dataset with replacement and a parameter is estimated on each sample. This procedure leads to a robust estimate of the true population parameter via sampling.

A model can then be trained on the data sample each bootstrap iteration and evaluated on the out of bag samples to give a performance statistic, which can be collected and from which confidence intervals may be calculated.

We can demonstrate this process with the following pseudocode.

```python
statistics = []
for i in bootstraps:
	train, test = select_sample_with_replacement(data, size)
	model = train_model(train)
	stat = evaluate_model(test)
	statistics.append(stat)
```

### Measuring

To measure your model properly you need to keep track of the model configuration, its (hyper-)parameters and the resulting metrics. One tool to help with that is [MLflow](https://github.com/mlflow/mlflow), in particular its tracking feature.

```python
import mlflow

# Log parameters (key-value pairs)
mlflow.log_param("num_dimensions", 8)
mlflow.log_param("regularization", 0.1)

# Log a metric; metrics can also be updated throughout the run
mlflow.log_metric("accuracy", model.accuracy)

# Log artifacts (output files)
mlflow.log_artifact("roc.png")
mlflow.log_artifact("model.pkl")
```

You use the tracking feature during your model's training run to update the experienced metrics. Afterwards when testing your model you can use these tracked metrics and test if the resulting model meets your defined level for each metric you care about.

For testing the measurements you can then use simple python testing frameworks like `unittest`. For the expected value of your tests it is common to use your baselines, e.g. the model must reach at least 80% of human level performance or it must meet better than the previous model.

> When doing these test, you can't expect the model to produce e.g. an exact accuracy value and predefine that in your tests. One should define the test as greater than or smaller than (which ever makes more sense).

### Examples

To find examples for these guidelines, go to the example repository: [MLOps pipeline](https://github.ibm.com/datascience-ibm/example-mlops-model-pipeline).

During the build process the tests in the `test/` folder are being run automatically:

```
unittest: clean lint type-check
	pipenv run coverage run --source src,training -m unittest discover -v -s ./tests -p test*.py
	pipenv run coverage report -m --fail-under 0
	pipenv run coverage html -d build/unittest-coverage
	pipenv run coverage json -o build/unittest-coverage.json --pretty-print
	pipenv run coverage erase
```

Also the code coverage is evaluated and also reported to a JSON file as scripted above (part of the `makefile`).
After the report is generated, it can be viewed in visualization tool.

```
upload.monitoring: test
	pipenv run aws s3 cp ./build/unittest-coverage.json s3://machine-learning-metrics/trainer/unittest-coverage/unittest-coverage-$(version).json
	pipenv run aws s3 cp ./build/test-coverage.json s3://machine-learning-metrics/trainer/test-coverage/test-coverage-$(version).json
``` 

In the example the reports are pushed to AWS S3 with the commands above. From there they can be accessed with AWS QuickSight, which is a reporting tool, that can read JSON data from a S3 location directly.
The same concept can be applied to all other metrics generated during the build or training of the model.