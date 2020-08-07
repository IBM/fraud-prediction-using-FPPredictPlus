# Introduction 

**This Code Pattern is part of a series on Red Hat Marketplace operator FP Predict Plus. Please refer to the `Prerequisites` section for `Getting Started`.**

### How to build a Machine Learning classification model using FP Predict Plus

**`Machine learning`** is a large field of study that overlaps with and inherits ideas from many related fields such as artificial intelligence. The focus of the field is learning, that is, acquiring skills or knowledge from experience. Most commonly, this means synthesizing useful concepts from historical data. As such, there are many different types of learning that you may encounter as a practitioner in the field of machine learning from whole fields of study to specific techniques.

**`Classification`** in machine learning and statistics is a supervised learning approach in which the computer program learns from the data given to it and make new observations or classifications. There are different types of classification like Binary Classification, Multi-Class Classification, Multi-Label Classification. In this tutorial, we will focus on `Binary classification` and the methodology can be extended for other types of classification.

Examples of binary classification problems include:

* Given an example, classify if it is spam or not.
* Given recent user behavior, classify as churn or not.
* Given recent transactions, classify as fraudulent or not.

**We will focus on predicting fraudulent transactions using historical data and demonstrate the automated process of building models using `FP Predict plus operator` from Red Hat Marketplace.**

When the reader has completed this code pattern, they will understand how to :

* Quickly set up the instance on OpenShift cluster for model building.
* Ingest the data and initiate the FP Predict Plus process.
* Build different models using FP Predict Plus and evaluate the performance.
* Choose the best model and complete the deployment.
* Generate new predictions using the deployed model.

# Architecture Diagram

![](https://github.com/IBM/fraud-prediction-using-FPPredictPlus/blob/master/images/architecture.png)

1. User logs into FP Predict Plus platform using an instance of FP Predict plus operator.
2. User uploads the data file in the CSV format to the Kubernetes storage on the platform.
3. User initiates the model building process using FP Predict Plus operator on OpenShift cluster and create pipelines.
4. User evaluates different pipelines from FP Predict Plus and selects the best model for deployment.
5. User generates accurate predictions by using the deployed model.

We will be using **`FP Predict Plus operator` from `Red Hat Marketplace`** to solve this usecase. Please refer to the content under **`Included components`** section to know more about `FP Predict Plus operator` and `Red Hat Marketplace`.

# Prerequisites

We need to install and set up the `FP Predict Plus operator on Open Shift cluster` as per the instructions given below.

[Install and setup FP Predict Plus operator on Red Hat Marketplace](https://github.com/IBM/getting-started-with-fppredictplus)

## Included components

* [Red Hat Marketplace](https://marketplace.redhat.com/en-us): A simpler way to buy and manage enterprise software, with automated deployment to any cloud.

* [FP Predict Plus](https://marketplace.redhat.com/en-us/products/fp-predict): An Automated, self learning, and Multi Modeling AI that handles Discrete Target variable, Continuous Target variable and Time series data with no need for coding.

* [Red Hat OpenShift Container Platform](https://www.openshift.com/): Empower developers to innovate and ship faster with the leading hybrid cloud, enterprise container platform.


## Featured technologies

* [Artificial Intelligence](https://developer.ibm.com/technologies/artificial-intelligence/): Any system which can mimic cognitive functions that humans associate with the human mind, such as learning and problem solving.

* [Data Science](https://developer.ibm.com/code/technologies/data-science/): Systems and scientific methods to analyze structured and unstructured data in order to extract knowledge and insights.

* [Analytics](https://developer.ibm.com/code/technologies/analytics/): Analytics delivers the value of data for the enterprise.


# Steps

Follow these steps to setup and run this code pattern using `FP Predict Plus`.

1. [Add the data](#1-add-the-data)
1. [Create a job](#2-create-a-job)
1. [Review the job details](#3-review-the-job-details)
1. [Analyze results](#4-analyze-results)
1. [Download the model file](#5-download-the-model-file)
1. [Prediction using new data](#6-prediction-using-new-data)
1. [Create predict job](#7-create-predict-job)
1. [Check job summary](#8-check-job-summary)
1. [Analyze results of predict job](#9-analyze-results-of-predict-job)
1. [Download predicted results](#10-download-predicted-results)

## 1. Add the data

Launch the FP Predict Plus platform and sign in using the default credentials. Lets begin by adding `datasets`. Clone this repo and navigate to `data` folder to download the datasets onto your local file system. 

Click on `Dataset Management` which is the third option on the left navigation pane and select `Datasets` on the top. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/add-data.png)

Click on `Browse` and select the three csv files for upload. The datasets gets uploaded to the platform in a minute. The upload time is dependent on the size of the datasets. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/data.png)

**Note :- Only `csv` format is supported and the dataset need to have a column with unique values. In these csv files, we have added a `Count` column to be unique. The datasets needs to be split into training, testing and holdout (validation) datasets before hand.** `Citation is needed to use these datasets for other projects.`

## 2. Create a job

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

## 3. Review the job details

Lets review the job which has been created for us by clicking on `Dashboard` which is the first option on the left navigation pane and select the job with name `detect-fraud`. The `number` preceding before the job name is to identify how many jobs have run so far and can be ignored. We can observe the `probability Distribution` of the model on the testing data having 50 records with the probability between `90%-100% for 43 records`. The model has performed with good accuracy on relatively small datasets. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/view-job.png)

We can observe the complete job details like `Description`, `Modelling` and `Prediction`. The system built 18 models using 505 records in 10 seconds from training data and did Prediction on 50 records from testing data.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/job-details.png)

## 4. Analyze results

Lets review the model performance in detail. Click on `Predicted vs Actual` option to see the Confusion matrix. The model has achieved 86% `Overall Accuracy` which is good considering the fact that training data had only 505 records & testing data had about 50 records. The accuracy can be expected to be higher for larger datasets. Avg `Precision` rate is about 84% and average `Recall` rate is about 80%. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/c-f-mtrx.png)

Click on `Models` to understand how many predictions are done by each model. We can observe that model `M-2` has scored for 29 records from the testing data.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/scoring-models.png)

Click on `Variables` to understand the significance of each variable in predicting the outcome. We can observe that `Credit_History_Available` was used by five models to predict the outcome thereby making it the most significant variable impacting the outcome. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/var-imp.png)

Click on `Variables of Models` to understand different scenarios explored by different models. We can observe that models `M-1` and `M-2` used four variables where as `M-3` used three variables and `M-7` used two variables for building different models. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/mdl-variables.png)

## 5. Download the model file

We can download the results which has all the model details mentioned above for further analysis. Lets go ahead and click on `Download Results` and `Download Model File` under `Download Files` option and save them in your local system. The `Results` and `Model File` for this experiment are also available in this repo under `reports` and `model` folders. The `Model file` can be uploaded onto cloud using the `Dataset Management` option as described earlier.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/dwld-r-m.png)

## 6. Prediction using new data

In this section, we will learn how to do predictions using the model on new dataset. We will be using the `saved model` from previous step to predict new records from the `holdout data`. The hold out data file will need to have the target variable column `Fraud_Risk` (without any values) to match the headers of the training data & predict the outcome given the transactions data. 

## 7. Create predict job

Lets create a new job for prediction by clicking on `Dashboard` option in the left navigation pane and hit `Start`. Update the `job name`, `description`, select `Predict` under `Tasks` as we have already built the model in previous steps. Upload the `model file` and `holdout data` from cloud or local whichever is convenient for you and select `Unique Identifier` as `Count`.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/prdt-frd.png)

The predict job will start per below. We should get a message stating `Job Completed Successfully` in a minute or two.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/job-prdt-frd.png)

## 8. Check job summary

Lets look at job summary by clicking `Dashboard` and selecting `predict-fraud` job. We can observe that, there are three records out of five having 90%-100% probability.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/job-chk.png)

## 9. Analyze results of predict job

We can get more details in the next step where we can observe that 18 models were built in 10 seconds and prediction was made on five records from the holdout dataset. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/prdt-frd-dtls.png)

`Note` :- Predicted vs Actual option is not clickable because there's no actual value to be compared. We have generated predicted values given a set of input parameters. We can review `Models, Variables & Variables of Models` as part of model evaluation.

## 10. Download predicted results

We can get all the details about model performance by clicking on `Download Results` under `Download Files` option per below. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/prdt-frd-dtls.png)

**We can observe predicted results for the new data in the second tab `Prediction Result` under `Predicted Value` attribute in the downloaded excel file. The `Results` file by name `predict-fraud-Report` is also available under `reports` folder for reference.**

# Summary

With this, we have come to an end of this code pattern. We have learnt how to use FP Predict Plus platform for building AI models using **`Classification`** technique and also explored how to churn out predictions on the new dataset. This platform will be beneficial for developers, data scientists to build AI solutions quickly under different domains.

# Citation for data :

The dataset which is referenced in this tutorial is prepared by R.K.Sharath Kumar, Data Scientist, IBM India Software Labs.
