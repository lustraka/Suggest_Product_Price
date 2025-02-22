# Predict House Prices in Ames, Iowa

This is a final project of the Udacity Machine Learning Engineer with Microsoft Azure Nanodegree Program. The project's goal is to train and deploy a machine learning model for **prediction of the sales price based on parameters of a house**. The project starts with importing a dataset from Kaggle. To find the best model, the project compares performance of models trained using Automated ML and one customized model whose hyperparameters are tuned using HyperDrive in the Microsoft Azure's ecosystem. The project wraps up with deploying the best model and testing its endpoint.

## Project Set Up and Installation

When an ML experiment is about to start, an ML engineer shall:
+ sign in to the Azure workspace, 
+ open Azure ML Studio,
+ create a compute instance to run Jupyter notebooks,
+ upload source code (Jupyter notebooks and Python scripts) from this GitHub repository.

## Dataset

### Overview

This project processes a data set describing the sale of individual residential property in Ames, Iowa from 2006 to 2010. The data set contains 2930 observations and a large number of explanatory variables (23 nominal, 23 ordinal, 14 discrete, and 20 continuous) involved in assessing home values. The Ames Housing dataset was compiled by Dean De Cock for use in data science education.

### Task

The project's goal is to train and deploy a machine learning model for prediction of the sales price based on parameters of a house. The `train_xgb.py` module encodes data types of categorical variables, imputes missing values and cleans the data in line with findings of an exploratory data analysis. There is no additional feature used in this iteration.

### Access

The [Ames Housing dataset](https://www.kaggle.com/c/house-prices-advanced-regression-techniques/data) is downloaded from Kaggle.com by `train_xgb.load_data_clean(source='kaggle')` and registered to the workspace. The HyperDrive child runs use the `train.csv` file saved locally after the download to prevent "Too many request" error from Kaggle (`train_xgb.load_data_clean(source='local')`).

## Automated ML

![AutoML Settings and configuration](http://www.plantuml.com/plantuml/png/RLLHRnit37xtho0mO6bJs5YxxcLeYmRB0WpOt64Dkhq9z8bd2zM9DubdrstttqVfT50G5qaSwyZl8_cHrBrDWb4IUbbzMMqsgzTmCmd_yVil77gtNtsv_su5htPlHemua524h_buH3_H_3MmqEeP2AQF-k0gqZvIIex3bHxbG23daO3xsKCOuXJKVunkm7WsIXfTbv61uu3UVIPh8hEe1OD9_quOzGd5o75Xh9YU8mnioKCYJvqupiGT3-CaP0JZdaGHx-zljhStVzsQsnVcmmlu7bwgZ-POoQe_LFyvmMAXJoDfbXG42hD4TC65lR4egEW3JudMewP2QOtsYA4Zksu2R3QR6cD1Ga4-wGbboXbWi83WSKV-QOwy8r1A2oSd1_Su2SsWU9gE_JG44t2WhDjxzFHdJk8AQhXYuNwXXXa0BbmciuBhQvo41syIYUZZwsiDrA8QqNyyQJjScAIezlp53_evu2StJ0Dak_AY07eFJYXYxaJAmkvS80iTQc3yI5gUjemUQwi8vKRbri31T95JCZfWpbQYh_1ZKVkaCSCPYWCqXooSaYdr0AS3XInXV6Wlehi4WGNII9th7TbSPC2T9qCO_TaauN7KyZNIVIopFPkXc9Sa2v_hImafJz72wuOnyZ7ZNP3PVE0u5xTwrcJvXfm_VLDCmkxGO4N3E8L5GlT8DiNhM8HkYZ7thE0MyLxgdoOpUQjeyAJYIPQ98v1kbmVXal2IGvY67SKyFu5jOyyhOrJdHgwyw-UWdofuRdvX6JabfxhL7LD0gwASkeFpTZrV0gywzXn_reKvlwdTu11oi65KH3wN1SzbFFIj7Rg40mNLfzC-b6JURRV_h1iPp6HvvYtrE7c1ElIfJFsyb-xpKjrLtenwwJmo9g5HrzJxhAdbG52sI8sTAd5MiZaJAWtNgip91SkdrEQhEW3ENVTkS08SvYJRbbxEWlx-FPELJUtyPCa43APiUukms22gQxbJO8jRA7TtMcw3Wb6U8yKT9oTdnUNzBiP59Kz7ykM-FKKdHLsUqRo7tdc29ufv_Hf-szwzUzfMItebkaEVSZQMnaWT2bbeRnFbiIwLrjrJI1pllEuv-Zr7pNVG2cSzZ5uO7ftIEgz9NcnfDI-Dtr6I-YAhUg8K0mLRRaRlgxa9tPQc5N1aKPFHp6ZGyqNO3yN-QpKt2cMmj9lsHWLQ1cdm6ptLSMhA2HTHQGQ0orCnB1TRoyHpXzuC671FjgcwgFTGqpKKPrYSunHIkK02YofJ6UqiYKjb_5eW6y93UYue5-P9kjrkOOT9mNKlxHHLW8lNy7_wyrQrJkZbVm00)

*Figure 1: Overview of the AutoML Configuration*

As there are time constraints for the experiment, the automl setting:

+ sets experiment timeout to 2 hours,
+ enables early stopping,
+ disables the neural networks (this is a default setting).

Number of cross validation is set to 3 as the dataset has more then 1.000 data points. For the sake of cross platform migration, the ONNX compatible models are enabled.

### Results

```
{'normalized_root_mean_squared_error': 0.03578930551264211,
 'root_mean_squared_log_error': 0.1302258721688295,
 'explained_variance': 0.8944570643792374,
 'normalized_root_mean_squared_log_error': 0.04236084835831457,
 'normalized_median_absolute_error': 0.014509132245016371,
 'root_mean_squared_error': 25771.878899653584,
 'median_absolute_error': 10448.026129636288,
 'spearman_correlation': 0.9562762595312719,
 'mean_absolute_error': 15609.99578903144,
 'mean_absolute_percentage_error': 9.19763905867747,
 'normalized_mean_absolute_error': 0.021677538937691213,
 'r2_score': 0.8942712633731941}
```

*Figure 2: The AutoML Best Run Metrics*

The results of the automated ML model presented in figure 2 has been achieved by a voting ensemble presented in figure 3. Further improvements of these results could have been achieved mainly by prolonging experiment's timeout and by including DNN based models into the loop.

![AutoML Voting Ensemble (click to see the image)](img/azml-3-automl-ensemble.png?raw=true)

*Figure 3: AutoML Voting Ensemble*

![AutoML RunDetails (click to see the image)](img/azml-4a-running-automl.png?raw=true)

---

![AutoML RunDetails (click to see the image)](img/azml-4b-running-automl.png?raw=true)

*Figure 4: AutoML RunDetails*

![The Best AutoML Model (click to see the image)](img/azml-5a-best-automl-model.png?raw=true)

---

![The Best AutoML Model (click to see the image)](img/azml-5b-best-automl-model.png?raw=true)

*Figure 5: The Best AutoML Model*

## Hyperparameter Tuning

The HyperDrive experiment uses XGBoost regressor which has been the best performing model from the voting ensemble crafted by the AutoML experiment. Hyperparameters tuned are:
+ `learing_rate` alias `eta` is the step size shrinkage used in update to prevent overfiffing, its range is [0,1] and for search are used values in interval [0.01, 0.2] with log scale,
+ `gamma` specifies the minimum loss reduction required to make a further partition on a leaf node of the tree; the larger `gamma` is, the more conservative the algorithm will be; its range is [0,∞] and for search is used interval [0,9]
+ `max_depth` is maximum depth of a tree, increasing this value will make the model more complex and more likely to overfit; its range is [0,∞] and for search are used values 3, 5, and 7.

These are Tree Booster parameters. Other parameters are left with their default values except a learning parameter `objective` which takes value `reg:squarederror`.

### Results

```
{'Learning rate': 0.06745453327900494,
 'Gamma': 7.785960174043349,
 'Maximum depth': 5.0,
 'r2_score': 0.9079294218993269}
```

*Figure 6: The HyperDrive Best Run Metrics*

Both the results of the HyperDrive tuning and corresponding parameters  of the XGBoost Regressor are presented in figure 6. Further improvements of these results could have been achieved mainly by increasing the number of total runs and by using Bayesian sampling instead of random parameter sampling.

![HyperDrive RunDetails (click to see the image)](img/azml-7a-running-hdr.png?raw=true)

---

![HyperDrive RunDetails (click to see the image)](img/azml-7b-running-hdr.png?raw=true)

---

![HyperDrive RunDetails (click to see the image)](img/azml-7c-running-hdr.png?raw=true)

*Figure 7: HyperDrive RunDetails*

![The Best HyperDrive Model (click to see the image)](img/azml-8-best-hdr-model.png?raw=true)

*Figure 8: The Best HyperDrive Model*


## Model Deployment

The best model has been deployed remotely on the CPU Cluster in line with MS guidelines "[Deploy machine learning models to Azure](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-deploy-and-where?tabs=python)". The XGBoost Regressor model tuned by HyperDrive has better R2 score than the voting ensemble of automated ML.

![The Best Model (click to see the image)](img/azml-8b-best-model.png?raw=true)

---

![Endpoint of the Model (click to see the image)](img/azml-9a-endpoint.png?raw=true)

---

![Endpoint of the Model (click to see the image)](img/azml-9b-endpoint.png?raw=true)

*Figure 9: Endpoint of the Model*

The endpoint of the model is consumed using Python SDK code as documented in the Python notebook.

## Screen Recording

[Screencast here](https://www.loom.com/share/43edfe51b6b8481eaa11987c8139e674?sharedAppSource=personal_library)

## Standout Suggestions

1. **ONNX**. The `ennable_onnx_compatible_models=True` parameter of `AutoMLConfig` ensures an ONNX model in the automated ML run outputs. There is no code for converting XGBoostRegressor to the ONNX format due to time constraints. So this standout is a bit half-baked.
2. Model deployment to the Edge using Azure IoT Edge was not attempted.
3. **Logging**. The deployment configuration (`AciWebservice.deploy_configuration()`) sets up the `enable_app_insights=True` parameter, so a web service enpoint can be monitored and data from it can be collected through Azure Application Insights.
4. [**Product Requirements Document**](prd/Readme.md) defines an Ames ML Experiment's requirements, including its purpose, features, functionality, and behavior. It offers a business view of the project.

## Backlog of Improvements
As the initial iteration of the Ames Housing Experiment if over, a machine learning engineer shall consider these improvements in future iterations:
+ Apply feature engineering. Carefully constructed features have a great potential to improve models' performance.
+ Enable deep learning with neural networks if there are more resources. Neural networks, when configured and trained properly, may have better performance than linear or tree-based models. It is at costs of explainability, though.
+ Use pipelines. Pipelines implement continuous integration, development, and testing to ensure consistent and quality code, that is readily available to users.
+ Use BayesianSampling in the HyperDrive tuning. Bayesian sampling tries to intelligently pick the next sample of hyperparameters, based on how the previous samples performed, such that the new sample improves the reported primary metric.
+ Use a PublishedPipeline, the pipeline to be submitted without the Pyton code which constructed it. The PublishedPipeline can be used to resubmit a pipeline with diferent parameter values and inputs and enabels "managed repeatability" in batch scoring and retraining scenarios.
<!--
![ (click to see the image)](img/.png?raw=true)
*Figure _: *
-->
