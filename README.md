# Concordia Big Data Course Project

This is a project for Concordia's Big Data class (SOEN691 UU) by Dr. Tristan Glatard in Winter 2020.
Team Members:

1. Le, Manh Quoc Dat (Student ID: 40153127)
2. Tran, Trong Tuan (Student ID: 40151694)
3. Phan, Vu Hong Hai (Student ID: 40154023)
4. Zhang, Yefei (Student ID: 40153319)
  
<!-- 
The team will work on both topics in this project: dataset analysis and algorithm implementation, for tackling an e-commerce recommendation system's problem. The dataset will be an open one from Kaggle or some other alternative open source.
  
There are some prominent candidate datasets: 
1. Elo Merchant Category Recommendation, Kaggle: https://www.kaggle.com/c/elo-merchant-category-recommendation 
2. Amazon Review Data 2018: https://nijianmo.github.io/amazon/index.html 
3. Goodreads Book Reviews: https://sites.google.com/eng.ucsd.edu/ucsdbookgraph/home 
  
Regarding the algorithms, there are a lot of them, and we need to choose based on the actual performance of experiments. We will try one method to be mentioned in the class like a collaborative filter, plus might be one that is not inside the class, and come up with a comparison in the evaluation of both methods, with a typical Root-Mean-Square Error (RMSE) as the indicator.
-->
  
## Abstract

The data from wearable gadgets (i.e. smartwatches or fitness tracking devices) is a reliable source that provides a lot of knowledge and potential to build a wide range of health-related applications such as health monitoring and physical training recommendation. Along with the increase in quantity and quality of data, the need to get the most out of data is more and more crucial. However, these data are heterogeneous, noisy, individual driven and sequential based, which are the key challenges to deal with. In this project, the main interests of our group are analyzing and building models that can predict people's heart rates based on sequential data from sensors. The dataset is collected from endomondo.com by the authors of [1], which contains about 200.000 records of over 1000 users with a couple hundred million of sensor measurements and metadata. In this project’s scope, we will develop machine learning (ML) models with the main concern about being able to process big time series data with 2 different approaches: one with the traditional methods learned in class and available in Spark, while another one to explore some methods a little bit more advanced, then compare them together.
  
## Introduction

The wearable gadgets such as smartwatches are becoming more and more popular nowadays. With many sensors, they can provide a lot of useful information about individual users as well as the environment. With appropriate data mining techniques, we can use these data for a larger potential application domain such as context-aware health supervisors. To achieve these such capabilities, there are several challenges that need to be overcome: (1) the collected data are sequential, type diversity and noisy, (2) the activity patterns and subjects' condition changes over time, and (3) the mining models need to be either small enough to be implemented on wearable gadgets or big enough to handle huge amounts of requests each time.
 Under the course's scope, we divide our work into 2 main folds following the typical framework:

1. Apply learned techniques to analyze and preprocess the dataset: the raw data contains lots of noisy metadata that need to rectify. We also need to calculate several derived features to improve the models' performance.
2. Implement ML models that can be scaled in Spark: We expect to implement at least two prediction models in Spark to predict the heart rate of individual users based on the context of the training such as historical workout, current heart rate, and user-dependent features. We are planning to implement non-deep learning models only for the sake of limited processing time.
  
Recently, there were several relevant works in mining sensor data for health area: [2] provided an overview of data mining research in healthcare and discussed the impact of these techniques on pervasive sensing market; Farseev et al. proposed a model that combined collected exercises and social network data to predict users’ wellness trend (Body Mass Index - BMI) using AdaBoost-based method [4]; [3, 24] built context-aware models that applied in many fields such as recommendation, social network and clinical predictions using individual information like BMI, age, gender, etc.
  
## Materials and Methods  
  
### Dataset

The dataset used in this project is collected by Ni et al. in [1] from [endomondo.com](https://sites.google.com/eng.ucsd.edu/fitrec-project/home). The data contains sensors data: heart rate, timestamps, distance, speed, ... and contextual data: GPS location (longitude and latitude), altitude, gender, sport (activity) and users' ID, ... The overview of this dataset is described in Table 1.
  
**Table 1**: Endomondo dataset overview
|Attribute|Quantity|
|---|---|
|Number of workouts|253,020|
|Total number of records|111,541,956|
|Number of users|1,104|
|Average length (hours)|5.998|
  
The dataset of each workout consists of 500 data points recorded through flexible time intervals from seconds to several minutes. Therefore, we need to deal with this kind of interval variation. In this project, after analyzing the data, our main goal is to predict the heart rate of each user over the course of the workout based on the sensors and contextual data. The heart rate prediction model then can help to warn users about the abnormal health condition and give recommendations about increasing or decreasing workout's intensity.
  
### Techniques and Algorithms

We consider data analysis as part of pre-processing data to train the models. We are expected to find some insights about data to selecting and deriving features, which can help improve the ML models' performance. The main pre-exploratory techniques will be used are:

1. Plotting and normalizing based on some distribution for each data type
2. Calculating data's common statistics: mean, standard deviation, etc.
3. Using the non-supervised techniques such as clustering to gain insight
4. Generating derived features, using interpolation and resampling technique to fill missing data points
  
After analyzing and preprocessing data, we will feed the data to our implemented algorithms, and base on PySpark as the main framework. Since the goal of our predictor is to predict the heart rate (beats per minute - BPM), the result can be acquired by two methods: (a) directly via a regression model, or (b) via a classification model with each specific value of BPM is considered as a label (considering about human heartbeats, there will be only maximum 200 labels).
 We will consider two approaches:

1. Using window technique: We will slice the data using a fixed time-window and apply the available ML algorithms in PySpark such as Random Forest Regression.
2. Processing sequential data as-is: There are various algorithms that can model sequential data. In this project, there are two candidates: Hidden Markov Model (HMM) and Conditional Random Field (CRF). These algorithms are very popular when dealing with such kind sequential data, especially in natural language processing like text and voice. Our group will implement these algorithms and make sure they will work with the Spark environment (maybe through another adaptor like Yahoo’s TensorFlowOnSpark).
Of course, with the non-deep learning methods, we do not expect our algorithms to outperform the original ones from the mainly referred article [1]. However, at least we can have a chance to compare the effectiveness of our 2 proposed approaches.  

### Evaluation

We will use RMSE as the main metric to evaluate our efforts. The training and testing dataset will be divided by each user's workout.

## Discussion

### Limitations

- Random Forest algorithm’s disadvantages [6]:
  - It takes a huge amount of computational costs and memory to train a large number of deep trees.
  - Predictions are slower so that it could put challenges and pressure on applications.
  - The model created by the Random Forest algorithm is less explainable than an individual decision tree.

### Future Work

   Due to limited time offered, we could not have applied as many algorithms as we had planned. We decided to put them into our future work so that we could have much time to analyzing to get deeper insights of the dataset as well as implementing variety of models using different types of algorithms related to Time Series, for example, Conditional Random Field (CRF) and Recurrent Neural Network algorithms.

   There is an additional data file that we had not analyzed yet because we recognized that the data from it do not contribute a lot in our current model. However, there are some attributes from that dataset that could be helpful in other algorithms, such as weather, so we definitely need to work on this dataset to build models that have higher accuracy utilizing the two above-mentioned algorithms.

   Finally, we really want to embed each user’s historical workout measurements into our models. The reason behind that is that we want to develop a system that is user-centered. The system could personalize each user’s workout sequential modeling to predict how each user’s heart rates will fluctuate across each workout. In addition, the ability of the system could also be reached to identify clusters of users that share the common embedding structures and recommend alternative routes for users to achieve a heart rate profile [1].

## References

[1]. Jianmo Ni, Larry Muhlstein, Julian McAuley, "Modeling heart rate and activity data for personalized fitness recommendation", in Proc. of the 2019 World Wide Web Conference (WWW'19), San Francisco, US, May. 2019.

[2]. Hadi Banaee, Mobyen Uddin Ahmed, and Amy Loutfi. 2013. Data Mining for Wearable Sensors in Health Monitoring Systems: A Review of Recent Trends and Challenges. In Sensors

[3]. Miguel Ramos de Araujo, Pedro Manuel Pinto Ribeiro, and Christos Faloutsos. 2017. TensorCast: Forecasting with Context Using Coupled Tensors (Best Paper Award). In ICDM

[4].  Aleksandr Farseev and Tat-Seng Chua. 2017. TweetFit: Fusing Multiple Social Media and Sensor Data for Wellness Profile Learning. In AAAI

[5].  Soujanya Poria, Erik Cambria, Devamanyu Hazarika, Navonil Majumder, Amir Zadeh, and Louis-Philippe Morency. 2017. Multi-level Multiple Attentions for Contextual Multimodal Sentiment Analysis. In ICDM

[6]. S. Jansen, Hands-On Machine Learning for Algorithmic Trading [[Book]](https://www.oreilly.com/library/view/hands-on-machine-learning/9781789346411/). Packt Publishing, 2018.
