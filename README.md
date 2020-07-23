# Introduction 

This tutorial is part of a series on Red Hat Marketplace operator FP Predict Plus. Please refer to the prerequisites section for **Getting Started**.

### How to build a Machine Learning classification model using FP Predict Plus

`Machine learning` is a large field of study that overlaps with and inherits ideas from many related fields such as artificial intelligence. The focus of the field is learning, that is, acquiring skills or knowledge from experience. Most commonly, this means synthesizing useful concepts from historical data. As such, there are many different types of learning that you may encounter as a practitioner in the field of machine learning: from whole fields of study to specific techniques.

`Classification` in machine learning and statistics is a supervised learning approach in which the computer program learns from the data given to it and make new observations or classifications. There are different types of classification like Binary Classification, Multi-Class Classification, Multi-Label Classification. In this tutorial, we will focus on `Binary classification` and the methodology can be extended for other types of classification.

Examples of binary classification problems include:

* Given an example, classify if it is spam or not.
* Given recent user behavior, classify as churn or not.
* Given recent transactions, classify as fraudulent or not.

We will be using **`FP Predict Plus operator` from `Red Hat Marketplace`** to solve this usecase. Please refer the links to know more about [FP Predict Plus](https://marketplace.redhat.com/en-us/products/fp-predict) and [Red Hat Marketplace](https://marketplace.redhat.com/en-us).

# Prerequisites

We need to install and set up the `FP Predict Plus operator on Open Shift cluster` as per the instructions given below.

[Install and setup FP Predict Plus operator on Red Hat Marketplace](https://github.com/IBM/getting-started-with-fppredictplus)

# Estimated time

It should take about 30-45 minutes to complete this tutorial.

# Steps

Launch the FP Predict Plus platform and sign in using the default credentials. Lets begin by adding `datasets`. Clone this repo and navigate to `data` folder to download the datasets onto your local file system. 

Click on `Dataset Management` which is the third option on the left navigation pane and select `Datasets` on the top. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/add-data.png)

Click on `Browse` and select the three csv files for upload. The datasets gets uploaded to the platform in a minute. The upload time is dependent on the size of the datasets. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/data.png)

**Note :- Only `csv` format is supported and the dataset need to have a column with unique values. In these csv files, we have added a `Count` column to be unique. The datasets needs to be split into training, testing and holdout (validation) datasets before hand.** `Citation is needed to use these datasets for other projects.`

We need to create a new `job` in the platform. Click on `Dashboard` which is the first option on the left navigation pane and hit `Start` on the top right hand side.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/job-strt.png)

We need to click on `No` as our data does not contain Date or Timestamps. The platform will understand to create a `Predict` job with no Date or Timestamps and if the dataset has Date or Timestamps, then platform will create a `Forecast` job.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/job-sel.png)

Lets go ahead and create a new job by filling in the details per below. We will update the name, select the task as `Model + Predict` as that is what we will be doing. We can select `Model` and it will build a model for us and select `Predict` if we have a model file ready for doing predictions. Set the dataset location to `Cloud` and select the train and test datasets using `Browse` for upload. Select the Target Variable as `Fraud_Risk` and Unique Identifier as `Count`. Under `Advanced settings`, Operation Mode should be set to `Automated`. Hit `Run` to get the job started.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/crt-job.png)

The job will take a couple of minutes to complete. We can observe how many models are created and the scenarios evaluated in the process. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/job-rng.png)

The model will try to use different scenarios like all variables/few variables etc for predicting the outcome. We should see the job status per below.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/job-complete.png)

Lets review the job which has been created for us by clicking on `Dashboard` which is the first option on the left navigation pane and select the job with name `detect-fraud`. The `number` preceding before the job name is to identify how many jobs have run so far and can be ignored. We can observe the `probability Distribution` of the model on the testing data having 50 records with the probability between `90%-100% for 43 records`. The model has performed with good accuracy on relatively small datasets. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/view-job.png)

We can observe the complete job details like `Description`, `Modelling` and `Prediction`. The system built 18 models using 505 records in 10 seconds from training data and did Prediction on 50 records from testing data.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/job-details.png)

Lets review the model performance in detail. Click on `Predicted vs Actual` option to see the Confusion matrix. The model has achieved 86% `Overall Accuracy` which is good considering the fact that training data had only 505 records & testing data had about 50 records. The accuracy can be expected to be higher for larger datasets. Avg `Precision` rate is about 84% and average `Recall` rate is about 80%. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/c-f-mtrx.png)

Click on `Models` to understand how many predictions are done by each model. We can observe that model `M-2` has scored for 29 records from the testing data.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/scoring-models.png)

Click on `Variables` to understand the significance of each variable in predicting the outcome. We can observe that `Credit_History_Available` was used by five models to predict the outcome thereby making it the most significant variable impacting the outcome. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/var-imp.png)

Click on `Variables of Models` to understand different scenarios explored by different models. We can observe that models `M-1` and `M-2` used four variables where as `M-3` used three variables and `M-7` used two variables. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/mdl-variables.png)


