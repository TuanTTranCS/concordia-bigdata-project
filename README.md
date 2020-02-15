# Concordia Big Data Course Project 

This is a project for Concordia's Bigdata class (SOEN691 UU) by Dr. Tristan Glatard in Winter 2020. 

Team Members: 

1. Le, Manh Quoc Dat (Student ID: 40153127) 

2. Tran, Trong Tuan (Student ID: 40151694)   

3. Phan, Vu Hong Hai (Student ID: 40154023) 

4. Zhang, Yefei (Student ID: 40153319) 

  

<!-- 

The team will work on both topics in this project: dataset analysis and algorithm implementation, for tackling of an e-commerce recommendation system's problem. The dataset will be a open one from Kaggle or some other alternative open sources.  

  

There are some prominent candidate datasets: 

1. Elo Merchant Category Recommendation, Kaggle: https://www.kaggle.com/c/elo-merchant-category-recommendation 

2. Amazon Review Data 2018: https://nijianmo.github.io/amazon/index.html 

3. Goodreads Book Reviews: https://sites.google.com/eng.ucsd.edu/ucsdbookgraph/home 

  

Regarding to the algorithms, there are a lot of them, and we need choose based on the actual performance of experiments. We will try one method to be mentioned in the class like collaborative filter, plus might be one that is not inside the class, and come up with a comparison in evaluation of both methods, with a typical Root-Mean-Square Error (RMSE) as the indicator. 

--> 

  

## Abstract 

The data from wearable gadgets (i.e. smartwatches or fitness tracking devices) is a reliable source that provide a lot of knowledge and potentials to build a wide range of health-related applications such as health monitoring and physical training recommendation. Along with the increase in quantity and quality of data, the need to get the most out of data is more and more crucial. However, these data are heterogeneous, noisy, individual driven and sequential based, which are the key challenges to deal with. In this project, the main interests of our group are analyzing and building models that can predict people's heart rates based on sequential data from sensors. The dataset is collected from endomondo.com by the authors of [1], which contains about 200.000 records of over 1000 users with a couple hundred million of sensor measurements and metadata. In this project’s scope, we will develop machine learning (ML) models with the main concern about being able to process big timeseries data with 2 different approaches: one with the traditional methods learnt in class and available in Spark, while another one to explore some methods a little bit more advanced, then compare them together. 

## Introduction
The wearable gadgets such as smartwatch are becoming more and more popular at the time. With many sensors, they can provide lots of useful information about each individual user as well as the environment. With approriate data mining techniques, we can use these data for a larger application domain such as context-aware health supervise. To achieve these such application, there are several challenges that need to overcome: (1) the collected data are sequential, various of types and noisy, (2) the activity patterns and subjects' condition changes over time and (3) the mining models are either need to be small enough to put on wearable gadgets or big enough to be able to handle huge amount of requests at a single time.

Under the course's scope, we divided our work to 2 main fold:
1. Apply learned technique to analyst and preprocessing the dataset: the raw data contains lots of noisy, metadata that need to rectify. We also need to calculate several derive features to improve the models' performance.
2. Implement machine learning models that can be scaled in Spark: We are expected to implement at least two prediction models in Spark to predict the heart rate of individual users given the context of the training such as historical workout, current heart rate, and user-dependent features. We are planning to implement non-deep learning models only for the sake of processing time.

Recently, there are several relevant works in mining sensor data for health area: [2] provides an overview of data mining research in healthcare and discussed about impact of these techniques to pervasive sensing market; Farsevv et al. proposed a model that combine collected exercise and social network data to predict user wellness trend (Body Mass Index - BMI) using AdaBoost-based method [4]; [3, 5] built context-aware models that applied in many fields such as recommendation, social network and clinical predictions using individual information like BMI, age, gender.

## Materials and Methods 

### Dataset
The dataset used in this project is collected by Ni et al. [1] from endomondo.com https://sites.google.com/eng.ucsd.edu/fitrec-project/home . The data contains sensors data: heart rate, timestamps, distance, speed, ... and contextual data: GPS location (longitude and latitude), altitude, gender, sport and users' ID, ... The details of the dataset described in Table 1.

**Table 1**: Endomondo dataset details
|Attribute|Quantity|
|---|---|
|Number of workouts|253,020|
|Number of users|1,104|
|Average length (hours)|5.998|

The data of each workout consist of 500 data points in flexible time intervals from seconds to several minutes. Therefore, we need to deal with these types of interval information. In this project, after analyst the data, our main goal is predict the heart rate of each user over the course of the workout based on the sensors and contextual data. The heart rate prediction model then can help to warn user about the abnormal health condition, for recommending an increase or decrease workout's intensity.

### Techniques and Algorithms
We consider analyst the data is a part of preprocessing data to train the models. We expected to find out some insight about data to selecting and deriving features, which can help improve the machine learning models' performance. The main analyst technique will be used are:
1. Plotting and normalizing based on some distribution
2. Calculate data's distribution: mean, deviation, ...
3. Using non-supervised techniques such as clustering to gain insight
4. Calculate derived features, using interpolation and resampling technique to fill the missing data point

After the analyst and preprocessing data, we will feed the data to our implemented algorithms. We will use PySpark as the main framework and implement algorithms based on it. Since the gold for our predictor is predict the hear rate (beat per minute - BPM), the result can be acquired by two methods: (a) directly via a regression model or (b) via a classification model with each amount of BPM is considered as a label (since we consider about human heart beat, there will be only maximum 200 labels).

We are considering these approachs:
1. Using window technique: We will slide the data using a fixed time-window and apply the implemented machine learning algorithm in PySpark such as regression and Random forest.
2. Process sequential data as it is: There are various algorithms that can model sequential data. In this project, there are two candidates: Hidden Markov Model (HMM) and Conditional Random Field (CRF). These algorithms are very popular when dealing with sequential data such, especially natural language like text and voice. Our group will need to implement these algorithms and make sure they will work with Spark environment.
Since we are comparing with deep learning algorithms, we are not expecting our algorithm to outperform the original ones.

### Evaluation
We will use RMSE as the main scale to evaluate our effort. The training and testing dataset will be divided by each user's workout.

## References
[1]. Jianmo Ni, Larry Muhlstein, Julian McAuley, "Modeling heart rate and activity data for personalized fitness recommendation", in Proc. of the 2019 World Wide Web Conference (WWW'19), San Francisco, US, May. 2019.

[2]. Hadi Banaee, Mobyen Uddin Ahmed, and Amy Loutfi. 2013. Data Mining for Wearable Sensors in Health Monitoring Systems: A Review of Recent Trends and Challenges. In Sensors

[3]. Miguel Ramos de Araujo, Pedro Manuel Pinto Ribeiro, and Christos Faloutsos. 2017. TensorCast: Forecasting with Context Using Coupled Tensors (Best Paper Award). In ICDM 

[4].  Aleksandr Farseev and Tat-Seng Chua. 2017. TweetFit: Fusing Multiple Social Media and Sensor Data for Wellness Profile Learning. In AAAI

[5].  Soujanya Poria, Erik Cambria, Devamanyu Hazarika, Navonil Majumder, Amir Zadeh, and Louis-Philippe Morency. 2017. Multi-level Multiple Attentions for Contextual Multimodal Sentiment Analysis. In ICDM
