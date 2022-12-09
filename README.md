# WARNING: This repository is no longer maintained

> This repository will not be updated. The repository will be kept available in read-only mode.

# Introduction 

**This code pattern is part of a series on Red Hat Marketplace operator FP Predict Plus. Please refer to the `Prerequisites` section for `Getting Started`.**

### How to build a machine learning classification model using FP Predict Plus

**Machine learning** is the science where in order to predict a value, algorithms are applied for a system to learn patterns within data. Most commonly, this means synthesizing useful concepts from historical data. Machine learning uses different types of learning, related to whole fields of study to specific techniques.

**Classification** in machine learning and statistics is a supervised learning approach in which the computer program learns from the data given to it and makes new observations or classifications. Different types of classification include *binary classification, multiclass classification,* and *multilabel classification*. In this tutorial, we focus on binary classification, but you can extend the methodology for other types of classification.

Examples of binary classification problems include:

* Classifying an example email as spam or not. 
* Classifying recent user behavior as churn or not.
* Classifying recent transactions as fraudulent or not.

**Specifically, this code pattern focuses on predicting fraudulent transactions using historical data and demonstrates the automated process of building models using `FP Predict plus operator` from Red Hat Marketplace.**

After completing this code pattern, you will understand how to:

* Quickly set up the instance on an OpenShift cluster for model building. <!--EM: The instance of FP Predict +?-->
* Ingest the data and initiate the FP Predict Plus process.
* Build different models using FP Predict Plus and evaluate the performance of those models.
* Choose the best model and complete the deployment.
* Generate new predictions using the deployed model.

# Architecture diagram

![](https://github.com/IBM/fraud-prediction-using-FPPredictPlus/blob/master/images/architecture_c.png)

1. User logs into FP Predict Plus platform using an instance of FP Predict plus operator.
2. User uploads the data file in the CSV format to the Kubernetes storage on the Red Hat OpenShift platform.
3. User initiates the model-building process using the FP Predict Plus operator on an OpenShift cluster and creates pipelines.
4. User evaluates different pipelines from FP Predict Plus and selects the best model for deployment.
5. User generates accurate predictions by using the deployed model.

In this code pattern, we show you how to use **FP Predict Plus operator from Red Hat Marketplace** in our steps. Please refer to the content under **Included components** section to know more about `FP Predict Plus operator` and `Red Hat Marketplace`.

## Prerequisites

You need to install and set up the FP Predict Plus operator on Open Shift cluster following the instruction in this tutorial: [Get started using Findability Platform Predict Plus on Red Hat Marketplace](https://developer.ibm.com/tutorials/get-started-findability-platform-predict-plus-red-hat-marketplace/).

## Included components

* [Red Hat Marketplace](https://marketplace.redhat.com/en-us): A simpler way to buy and manage enterprise software, with automated deployment to any cloud
* [FP Predict Plus](https://marketplace.redhat.com/en-us/products/fp-predict): An sutomated, self learning, and multi-modeling AI tool that handles discrete target variables, continuous target variables, and time series data with no need for coding
* [Red Hat OpenShift Container Platform](https://www.openshift.com/): A hybrid cloud, enterprise container platform that empowers developers to innovate and ship faster


## Featured technologies

* [Artificial intelligence](https://developer.ibm.com/technologies/artificial-intelligence/): Any system which can mimic cognitive functions that humans associate with the human mind, such as learning and problem solving.
* [Data science](https://developer.ibm.com/code/technologies/data-science/): Systems and scientific methods to analyze structured and unstructured data in order to extract knowledge and insights.
* [Analytics](https://developer.ibm.com/code/technologies/analytics/): Analytics delivers the value of data for the enterprise.
* [Machine learning](https://www.ibm.com/cloud/learn/machine-learning): Machine learning is a form of AI that enables a system to learn from data rather than through explicit programming. 

# Steps

Follow these steps to set up and run this code pattern using FP Predict Plus.

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

Launch the FP Predict Plus platform and sign in using the default credentials. Let's begin by adding `data sets`. Clone this repo and navigate to the `data` folder to download the data sets onto your local file system. 

Click on **Dataset Management** which is the third option on the left navigation pane and select **Datasets** on the top. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/add-data.png)

Click on **Browse** and select the three CSV files for upload. It takes about 1 minute to upload the data sets to the platform, depending on the size of the data sets. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/data.png)

**Note :Only the .csv format is supported, and the data set needs to have a column with unique values. In these CSV files, we added a `Count` column to be unique. The data sets need to be split into training, testing, and holdout (validation) data sets before hand.** 

Citation is needed to use these data sets for other projects. <!--EM: What does that mean?-->

## 2. Create a job

Now, let's create a new job in the platform. Click on **Dashboard**, which is the first option on the left navigation pane, and click **Start** on the top right hand side.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/job-strt.png)

Select **No** to indicate that your data does not contain date or timestamps. The platform will understand to create a `Predict` job with no date or timestamps. If the data set has a date or timestamp, the platform will create a `Forecast` job.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/job-sel.png)

Now, let's go ahead and create a new job by filling in the details. Update the name and select the task as **Model + Predict** since that is the task you will be doing. If you select **Model**, it<!--EM: What is "it" referencing here?--> will build a model for us. Select **Predict** if you have a model file ready for doing predictions. Set the data set location to **Cloud**, and select the "Train and Test" data sets using the Browse function for upload. Set the Target Variable to be **Fraud_Risk** and the Unique Identifier to be **Count**. Under the Advanced Settings tab, set the Operation Mode to **Automated**. Select **Run** to start the job.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/crt-job.png)

The job will take a couple of minutes to complete. You can observe how many models are created and the scenarios evaluated in the process. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/job-rng.png)

The model will try to use different scenarios, for instance, all variables or few variables, for predicting the outcome. You should see the job status per below. <!--EM: Does that mean that you should see it when it's stopped running?-->

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/job-complete.png)

## 3. Review the job details

Let's review the job details. Click the **Dashboard** tab, and select the job with the name `detect-fraud`. The number that precedes the job name identifies how many jobs have run so far. You an ignore this number. 

In our example, the `probability distribution` of the model on the testing data has 50 records with the probability between `90%-100% for 43 records`. <!--EM: What does that mean exactly?--> The model has performed with good accuracy on relatively small data sets. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/view-job.png)

You can observe the complete job details, including the description, modeling, and prediction. The system built 18 models using 505 records in 10 seconds from the training data and ran predictions on 50 records from the testing data.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/job-details.png)

## 4. Analyze results

To review the model performance in detail, select the **Predicted vs Actual** option to see the Confusion matrix. The model has achieved 86% "Overall Accuracy" which is a high percentage, especially considering the fact that the training data had only 505 records and the testing data had approximately 50 records. You can expect the accuracy to be higher for larger data sets. The average Precision rate is about 84%, and the average Recall rate is about 80%. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/c-f-mtrx.png)

Click on **Models** to understand how many predictions each model does. Notice that model `M-2` has scored for 29 records from the testing data.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/scoring-models.png)

Select **Variables** to understand the significance of each variable in predicting the outcome. Notice that five models used "Credit_History_Available" to predict the outcome, which makes it the most significant variable to impact the outcome. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/var-imp.png)

Select **Variables of Models** to understand different scenarios explored by different models. Observe that models `M-1` and `M-2` used four variables, whereas `M-3` used three variables and `M-7` used two variables for building different models. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/mdl-variables.png)

## 5. Download the model file

To download the results for further analysis of all the model details, click on **Download Files >  Download Results > Download Model File**. Save the files to your local system. The Results and Model File for this experiment are also available in this repo under the `reports` and `model` folders. You can upload the Model file to a cloud using the `Dataset Management` option as described earlier.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/dwld-r-m.png)

## 6. Prediction using new data

In this section, learn how to do predictions using the model on a new data set. We use the `saved model` from the previous step to predict new records from the `holdout data`. In the hold out data file, the target variable column `Fraud_Risk` (without any values) needs to match the headers of the training data and predict the outcome given the transactions data. 

## 7. Create predict job

To create a new job for prediction, select **Dashboard** in left navigation pane and click **Start**. Update the job name and description. Under Tasks, select **Predict** since you already built the model in the previous steps. Upload the `model file` and `holdout data` from you cloud or local system (whichever is most convenient) and set the Unique Identifier to be "Count".

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/prdt-frd.png)

The predict job will start per below. You should get a message stating `Job Completed Successfully` in a minute or two.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/job-prdt-frd.png)

## 8. Check job summary

To view the job summary, click **Dashboard** and select the `predict-fraud` job. There are three records out of five that have a 90%-100% probability of predicting the outcome.

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/job-chk.png)

## 9. Analyze results of predict job

We can get more details in the next step where we can observe that 18 models were built in 10 seconds and prediction was made on five records from the holdout dataset. <!--EM: To clarify, should we say something like: You can now analyze the results from the job summary above. Note that 18 models were built in 10 seconds . . . . -->

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/prdt-frd-dtls.png)

`Note` :- Predicted vs Actual option is not clickable because there's no actual value to be compared. We have generated the predicted values given a set of input parameters. You can review models, variables, and variables of models as part of the model evaluation.

## 10. Download predicted results

To view the comprehensive data about model performance, select **Download Files > Download Results** as the followign screenshot shows. 

![](https://github.com/IBM/build-a-classification-model-using-fppredictplus/blob/master/images/prdt-frd-dtls.png)

**We can observe predicted results for the new data in the second tab `Prediction Result` under `Predicted Value` attribute in the downloaded Excel file. The Results file named "predict-fraud-Report" is also available under the Reports folder for reference.**

# Summary

With this, we have come to an end of this code pattern. We have learnt how to use FP Predict Plus platform for building AI models using **`Classification`** technique and also explored how to churn out predictions on the new dataset. This platform will be beneficial for developers, data scientists to build AI solutions quickly under different domains.

# Citation for data :

The dataset which is referenced in this tutorial is prepared by R.K.Sharath Kumar, Data Scientist, IBM India Software Labs.
