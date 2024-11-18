Lab 1 Description:

The goal of lab 1 was to build and deploy a serverless ML regression prediction system using Github and Hopswork. The pipeline uses air quality data from the website [Aqicn](https://aqicn.org/city/sweden/stockholm-st-eriksgatan-83/) and [Open-meteo](https://open-meteo.com/en/docs/air-quality-api). We specifically used the data from St-Eriksgatan-83 in Stockholm. From Aqicn we used the air quality metric pm2.5 data as the true values/labels and from Open-Meteo, temperature, wind speed, wind direction, and precipitation as our input values for our model. Using that data as training this model can forecast daily for the next 7 days from its training and has hindcast graphs showing the performance of the predictions. The model also uses lagged features to make the predictions more accurate. With the use of Github actions data is downloaded and used to train and then predict once per day.



[To view our prediction check this dashboard](https://hishamad.github.io/mlfs-book/air-quality/)

Part 01: Feature Backfill for Air Quality Data
In this part of the lab, the goal is to download the required data and upload it as a feature group to Hopsworks. The first step is to download the air quality data containing the daily pm2.5 measurements and the historical weather data. The second step is to do data cleaning which includes removing any rows that contain missing values(Nan values). The third step is to prepare the air quality and weather data frames. Here is a snapshot of both dataframes: 
![Dataframes](images/dataframes.png)
Part 02: Feature Pipeline
The goal of the feature pipeline is to automate the daily updates of our air quality and weather data as feature groups in Hopsworks. This pipeline also processes the latest data and cleanses inconsistent values to make sure our data is clean. Here we also include lagged features for the previous 3 days of pm2.5 values. Then the pipeline should register the air quality feature group and weather data feature group in Hopsworks.

Part 03: Training Pipeline
This pipeline trains a machine learning regression model to predict pm2.5 values using previous feature groups. The data is split into training and testing datasets based on a date threshold, which should then be trained into an XGBoost regression model with weather metrics and lagged features as input. Once the model is trained it is registered in Hopsworks Model Registry. 

Part 04: Prediction
The last pipeline performs daily predictions and visualizes them. Firstly the model from the previous step is retrieved and from that, our model predicts the next 7-10 days of pm2.5 values from batch inferred data. Lastly, the results are visualized by forecasts of pm2.5 values with dates and a hindcast graph showing the actual measured values next to the predicted.
