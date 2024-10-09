



[Uploadi
# NYC Yellow Taxi Trip Data Analysis (January 2023)

This project involves an in-depth analysis of New York City's Yellow Taxi trip data for January 2023. The notebook explores trip patterns, distances, fare amounts, and much more using various data science techniques.

## Project Overview

The aim of this project is to:
- Load and explore the dataset.
- Perform data cleaning and preprocessing.
- Conduct exploratory data analysis (EDA).
- Visualize insights from the data.

## Table of Contents

- [Installation](#installation)
- [Data](#data)
- [Steps in the Analysis](#steps-in-the-analysis)
- [Contributing](#contributing)
- [License](#license)

## Installation

Before running this notebook, make sure you have the following libraries installed:

```bash
pip install pandas numpy matplotlib seaborn
```

Additionally, you will need to download the **NYC Yellow Taxi Trip Data** from the official [NYC Taxi & Limousine Commission website](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page) for January 2023 in `.parquet` format.

## Data

The dataset contains various fields related to taxi operations, including:
- **VendorID**: A code indicating the provider associated with the trip record.
- **tpep_pickup_datetime**: Timestamp when the trip began.
- **tpep_dropoff_datetime**: Timestamp when the trip ended.
- **passenger_count**: Number of passengers in the trip.
- **trip_distance**: Distance traveled by the taxi.
- **fare_amount**: Fare for the trip.
- **total_amount**: Total fare paid, including taxes, surcharges, and tips.

## Steps in the Analysis

### 1. Data Loading

We start by loading the January 2023 taxi trip data stored in a `.parquet` file. This format is chosen for its efficiency in handling large datasets.

```python
import pandas as pd
taxi_jan_2023 = pd.read_parquet('yellow_tripdata_2023-01.parquet')
```

### 2. Data Exploration

In this step, we explore the dataset to understand its size, structure, and content. We inspect the shape, column names, and types of the data to ensure everything is as expected.

```python
taxi_jan_2023.shape  # Check the number of rows and columns
taxi_jan_2023.columns  # Display all columns
```

A snapshot of the data is displayed for a better understanding:

| tpep_pickup_datetime | tpep_dropoff_datetime | passenger_count | trip_distance | total_amount |
|----------------------|-----------------------|-----------------|---------------|--------------|
| 2023-01-01 00:32:10  | 2023-01-01 00:40:36   | 1.0             | 0.97          | 14.30        |
| 2023-01-01 00:55:08  | 2023-01-01 01:01:27   | 1.0             | 1.10          | 16.90        |
| 2023-01-01 00:25:04  | 2023-01-01 00:37:49   | 1.0             | 2.51          | 34.90        |

### 3. Data Cleaning

Before proceeding with analysis, it's important to clean the data. This includes:
- Handling missing values.
- Converting data types (e.g., datetime columns).
- Filtering invalid or erroneous data (e.g., trips with zero distance).

```python
taxi_jan_2023.dropna(inplace=True)  # Remove rows with missing values
taxi_jan_2023['tpep_pickup_datetime'] = pd.to_datetime(taxi_jan_2023['tpep_pickup_datetime'])
taxi_jan_2023['tpep_dropoff_datetime'] = pd.to_datetime(taxi_jan_2023['tpep_dropoff_datetime'])
```

### 4. Exploratory Data Analysis (EDA)

In this section, we use descriptive statistics and visualizations to understand the distribution of key variables like trip distances, passenger counts, and fare amounts.

**Trip Distance Distribution**  
We create histograms and box plots to show the distribution of trip distances.

```python
taxi_jan_2023['trip_distance'].hist(bins=50)
```

The plot shows that most trips are within a short distance range (1-2 miles).

**Fare Distribution**  
Similarly, we visualize how taxi fares are distributed across the dataset:

```python
taxi_jan_2023['total_amount'].hist(bins=50)
```

### 5.Feature engineering
I’ve added 3 sets of new features to the model. First set is time-based feature. These include, weekend and holiday boolean.

Second set is location-based information. We have Location IDs per region but there is a higher level abstraction for regions called Boroughs. This information came from the source of the main data.

The last set is weather related data. I’ve downloaded this data from this website. It’s free to use for development.

Here is a list of all features used in the final model: ['PULocationID', 'transaction_month', 'transaction_day', 'transaction_hour', 'transaction_week_day', 'weekend', 'is_holiday', 'Borough’, 'temperature', 'humidity', 'wind speed', 'cloud cover', 'amount of precipitation’, ‘total_amount’]


The algorithms I tried and the results
I tried Decision Trees, Random Forest and Gradient Boosting. The benchmark model is a Decision Tree. In the benchmark model, I only included the original features of the model as stated above. And on the normal models I used all original features plus the newly created ones.

Here are the performance results before tuning:

| Algorithm  | MAE  | RMSE | R2  |  
|------------------|----------|---------|-----------|
| Benchmark model	  |  11.7 | 17.6 |  0.27 |
|  Decision tree	 |  10.1 | 3.1	  |  0.37 |
| Random forest	  |  9.06 |  3.01 | 0.43  |
|  Gradient boosting	 |  10.02	 |  3.16	 |  0.44 |


The Random Forest model is selected to be tuned. Here are the best parameter values: n_estimators: 200 max_features': 'auto' 'max_depth': 150   bootstrap: True

The performance compares to previous models is:

|  Algorithm | MAE  | RMSE  | R2  |
|------------|--------|-------|----------|
|  Tuned Random Forest | 9.07  | 3.01  | 0.42  |
			

## Contributing

Feel free to submit issues or pull requests if you would like to improve this project.

## License

This project is licensed under the MIT License.
ng README.md…]()



