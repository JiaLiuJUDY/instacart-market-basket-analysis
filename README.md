# Instacart Market Basket Analysis

This repository contains my solution for the Instacart Market Analysis Competition hosted on kaggle. It helped me earn 39/2669 place. For anyone who is interested, please check [this page](https://www.kaggle.com/c/instacart-market-basket-analysis) for details about the Instacart competition.

## Dataset 

[the Instacart dataset](https://tech.instacart.com/3-million-instacart-orders-open-sourced-d40d29ead6f2) and can be used in other e-commerce datasets by modifying the input easily. 

## Solution

The problem is formulated as approximating P(u,p | user's prior purchase history) which stands for how much likely user u would repurchase product p given prior purchase history.

The main model should be a binary classifer and features created manually or automatically are feed to the classfier for generating predictions.

### Features
I construted both manual features and automatic features using unsupervised learning and neural networks.

Manual features include statistics of prior purchase history. As for automatic features, I used the following 

> LDA
> * each user is treated as a document and each product is treated as a word. 
> * generate topic representation of each user and each product
> * calculate score by taking inner product for <u,p> 
> * similar operations on aisle and department level
> * both score <u,p> and compressed topic representations of users/items serves as good features
> WORD2VEC
> * similar as LDA, but only on product level
> LSTM
> * Interval between user u's sequential purchase of product p is modeled as a time sequence.
> * Use LSTM to construct regression model for predicting next value of this time sequence.
> * The predicted next interval serves as a good feature.
> DREAM
> * RNN and bayesian personalized rank based Model, refer to this repo for my implementation
> * DREAM provides <u,p> scores, dynamic user representaions and item embeddings.
> * It captures the sequential information such as periodicity in users' prior purchase history.

### Classifier

I constructed both lightgbm model and xgboost model. 

### Optimization

I used bayesian optimization to tune my lightgbm model.

### Post-classification

I used [this script](https://www.kaggle.com/tarobxl/parallel-version-of-faron-s-script/) to contruct orders from <u,p> pair. Thanks to faron, shing and tarbox !

### Ensemble

I trained big models (500+ features), median models(260 + features) and small models(80+ features). Final submissions were generated by bagging top models using median.

## Files

Python Files

> `bayes_optim_lgb.py`
> * lightGBM model tuned by bayesian optimization
> `lgb_cv.py`
> * lightGBM model 5-fold cv
> `xgb_train_eval_test.py`
> * xgboost model 
> `transactions.py`
> * craft features manually from raw transaction log/user purchase history
> `feats.py'
> * combine all features and make train/test dataset
> `inference.py`
> * construct orders using P(u,p) 
> `evaluation.py`
> * some functions related to local evaluation
> `constants.py`	
> * some constants such as file path
> `utils.py`
> * some useful functions

Jupyter notebbooks

> `EDA and Feat Craft`
> * dataset exploration and feature crafting
> `Evaluation and Bagging`
> * local evaluation and bagging models
> `Submission and Bagging`
> * generate submissions

## License

Copyright (c) 2017 Yihong Chen