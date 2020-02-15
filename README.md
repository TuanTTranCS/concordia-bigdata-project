# Concordia bigdata project
This is a project for Concordia's Bigdata class (SOEN691 UU) by Dr. Tristan Glatard in Winter 2020.
Team Members:
1. Le, Manh Quoc Dat (Student ID: 40153127)
2. Tran, Trong Tuan (Student ID: 40151694)  
3. Phan, Vu Hong Hai (Student ID: 40154023)
4. Zhang, Yefei (Student ID: 40153319)

<!--
The team will work on both topics in this project: dataset analysis and algorithm implementation, for tackling of an e-commerce recommedation system's problem. The dataset will be a open one from Kaggle or some other alternative open sources. 

There are some prominent candidate datasets:
1. Elo Merchant Category Recommendation, Kaggle: https://www.kaggle.com/c/elo-merchant-category-recommendation
2. Amazon Review Data 2018: https://nijianmo.github.io/amazon/index.html
3. Goodreads Book Reviews: https://sites.google.com/eng.ucsd.edu/ucsdbookgraph/home

Regarding to the algorithms, there are a lot of them and we need choose based on the actual performance of experiments. We will try one method to be mentioned in the class like collaborative filter, plus mightbe one that is not inside the class, and come up with a comparison in evaluation of both methods, with a typical Root-Mean-Square Error (RMSE) as the indicator.
-->

## Abstract
The data from wearable gadgets (i.e. smart watch) is a reliable source that provide lots of knowledge about substantial range of application such as health monitoring, physical training recommendation. Along with the increasing of quantity of data and quality, the need of getting most out of the data is more and more necessary. However, these data are heterogeneous, noisy, individual driven and sequential-based, which are the key challenger to process. In this project, the main interests of our group are analyzing and building models that can predict people's heart rates based on sequential data from sensors. The dataset is collected from endomondo.com by the authors of [], which contains about 200.000 records of over 1000 users with couple hundred million of sensor measurements and metadata. In the scope of this project, we are developing machine learning models with the main concern about being able to process timeseries big data.

## Introduction
The wearable gadgets such as smart watch are becoming more and more pupular at the time. With many sensors, they can provide lots of useful information about each individual user as well as the environment. With approriate data mining technique, we can use these data for a larger application domain such as context-aware health suppervise. To achive these such application, there are several challenges that need to overcome: (1) the collected data are sequential, various of types and noisy, (2) the activity patterns and subjects's condition changes over time and (3) the mining models are either need to be small enough to put on wearable gadgets or big enough to be able to handles huge amount of requests at a single time.

Under the course's scope, we devided our work to 2 main fold:
1. Apply learned technique to analyst and preprocessing the dataset: the raw data contains lots of noisy, meta data that need to rectify. We also need to calculate several derive features to improve the models' performance.
2. Implement machine learning models that can be scale in Spark: We are expected to implement at least two predition model in Spark to predict the heart rate of individual users given the context of the training such as historical workout, current heart rate and user-dependent features. We are plaining to impliment non-deep learning model only for the sake of processing time.

Recently, there are several relevent work in mining sensor data for health area: [4] provides an overview of data mining research in healthcare and discussed about impact of these technique to pervasive sensing market; Farsevv et al. proposed a model that combine collected execise and social network data to predict user wellness trend (Body Mass Index - BMI) using AdaBoost-based method [9]; [8, 24] built context-aware models that applied in many fields such as recommendation, social network and clinical predictions using invidual information like BMI, age, gender.

## Materials and Methods 

### Dataset
The dataset used in this project is collected by Ni et al. in [] from endomondo.com. The data contains sensors data: heart rate, timestamps, distance, speed, ... and contextual data: gps location (longtitude and latitude), altitude, gender, sport and users' ID, ... The details of the dataset descibed in Table 1.

**Table 1**: Endomondo dataset details
|Attribute|Quantity|
|---|---|
|Number of workouts|253,020|
|Number of users|1,104|
|Average length (hours)|5.998|

The data of each workout consist of 500 data points in flexible time intervals from seconds to serveral minutes. Therefore, we need to deal with these type of interval information. In this project, after analyst the data, our main goal is predict the heart rate of each user over the course of workout based on the sensors and contextual data. The heart rate prediction model then can help to warning user about the abnormal health condition, for recommending increase of decrease workout's intensivity.

### Techniques and Algorithms
We consider analyst the data is a part of preprocessing data to train the models. We exptected to find out some insight about data to selecting and deriving features, which can help improve the machine learning models' performance. The main analyst technique will be used are:
1. Plotting and normalizing based on some distribution
2. Calculate data's distribution: mean, deviation, ...
3. Using non-supervised technique such as clustering to gain insight
4. Calculate derived features, using interpolation and resampling technique to fill the missing data point

After analyst and preprocessing data, we will feed the data to our implemented algorithms. We will used PySpark as the main framework and implement algorithms based on it. Since the gold for our predictor is predict the hear rate (beat per minute - BPM), the result can be accquired by two methods: (a) directly via a regression model  or (b) via a classification model with each amount of BPM is considered as a lable (since we consider about human heart beat, there will be only maximum 200 labels).

We are considering these approach:
1. Using window technique: We will slide the data using a fixed time-window and apply the implemented machine learning algorithm in PySpark such as regression and Random forest.
2. Process sequential data as it is: There are various algorithms that can model sequential data. In this project, there are two candidates: Hidden Markov Model (HMM) and Conditional Random Field (CRF). These algorithms are very popular when dealing with sequential data such, especially natural language like text and voice. Our group will need to implement these algorithms and make sure they will work with Spark environment.
Since we are comparing with deep learning algorithms, we are not expect our algorithm to out perform the original ones.

### Evaluation
We will use the RMSE as the main scale to evaluate our effort. The training and testing dataset will be devided by each user's workout.
