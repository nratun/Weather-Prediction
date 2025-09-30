# Final Report

## Introduction
This project creates a weather parameter prediction model using two different machine learning algorithms--Random Forest, and LSTM (Long Short-Term Memory). Using 10 days of weather data from a real Mesonet weather station, both models are evaluated to compare and contrast performance.

## Overview
For this project, I used the Random Forest as the traditional ML model for Part 1, and the Long Short-Term Memory (LSTM) model for Part 2. Part 1 involved predicting the next hour's weather conditions given the past six hours of data, while Part 2 involved predicting the next three hours of weather conditions given the past twelve hours of data.

## Functionality of Models
### Random Forest
Random Forest trains multiple decision trees and averages their outputs to make predictions, which makes it a popular and effective choice for handling non-linear relationships and noisy data effectively. However, it's known to struggle with sequential patterns, which are crucial for weather data prediction. Because of this, the model doesn't perform as well as LSTM for the purpose of the project.

### LSTM
Unlike Random Forest, LSTM excels in handling sequential data by using memory cells to retain information over time. It's particularly beneficial for weather data prediction due to its ability to model time-series dependencies and patterns, making it more suited for tasks involving long-term dependencies compared to traditional machine learning models.

## Model Implementation
### Random Forest
- Data was flattened into 2D arrays to pass into the model

- A separate sliding-window method approach was used to ensure that the data was being input as chunks

- The regressor was trained with 100 estimators

- Performance was assessed using Mean Squared Error (MSE), Mean Absolute Error (MAE), R-squared (R²), and Root Mean Squared Error (RMSE)

### LSTM
- LSTM was implemented using four layers (50 units each), with the last layer returning sequences only for intermediate layers

- A dropout layer (50%) was used to mitigate overfitting

- Dense layers were added, followed by an output layer reshaped to match the target dimensions

- The model was trained over **25** epochs with a batch size of **64**

- Adam optimizer was used with a learning rate of **0.0001**

- Loss, Mean Absolute Error (MAE), and RMSE were calculated for both training and validation sets

## Data Preparation
The dataset contained 10 days of minute-by-minute weather data from a Mesonet weather station. Because the dataset had 75 attributes, most of which were unnecessary in predicting temperature, humidity, and pressure, I went through the feature descriptions and chose the features most related to the task at hand.

In the end, I ended up choosing the following eight attributes for training and testing:
- **Temperature:**  AirT_1pt5m, AirT_2m, AirT_9pt5m, AirT_10m

  - Air Temperature (1.5m)
  - Air Temperature (2m)
  - Air Temperature (9.5m)
  - Air Temperature (10m)

- **Humidity:** RH_2m, RH_10m

  - Relative Humidity (2m)
  - Relative Humidity (10m)

- **Pressure:** Pressure_1, Pressure_2

  - Air Pressure (1)
  - Air Pressure (2)

## Data For Training & Testing
1. **Feature Selection:** I excluded irrelevant and redundant features to retain only those directly related to temperature, humidity, and pressure

2. **Scaling:** I applied a MinMaxScaler to normalize the data to a range of 0 to 1 for both the Random Forest and LSTM models

3. **Data Chunking:** I chunked the data into groups based off how many hours needed to be input and predicted as mentioned by the project description:
    - **Random Forest:** Data processed in 6-hour chunks to predict the next hour by taking into consideration that each row counted as one minute
      - 6 hours = 360 minutes = 360 rows for input
      - 1 hour = 60 minutes = 60 rows for predicting the next hour

    - **LSTM:** Data processed in 12-hour chunks to predict the next three hours
      - 12 hours = 720 minutes = 720 rows for input
      - 3 hours = 180 minutes = 180 rows for predicting the next three hours

- *Note: The amount of chunks I created was a smaller amount than the total amount of chunks that could've been created from the data. This is because too many chunks made the models take too long to to run.*

4. **Data Splitting:** Data was divided into training **(80%)** and testing **(20%)** sets without shuffling to preserve the sequence of minutes within the hours

## Mean Square Error (and other Metric) Results
### Random Forest
Random Forest didn't have great results, most likely because it struggled with the sequential nature of the data. Additionally, the model required feature flattening.

- **MSE:** 0.0200
- **MAE:** 0.1045
- **R²:** -0.4892
- **RMSE:** 0.1415

### LSTM
LSTM significantly outperformed Random Forest, demonstrating its stength in handling sequential data. Additionally, the model preserved the structure of the data.

- **Loss:** 0.0071
- **MAE:** 0.0579
- **RMSE:** 0.0840