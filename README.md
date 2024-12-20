# Python-Tableau-London-Bike-Sharing-Project
![Logo](https://github.com/AkarshGM/Python-Tableau-London-Bike-Sharing-Project/blob/main/Tableau-python%20logo.png)


## Objective
This Project's objective is to gather data programatically from kaggle, assess and explore the data, manipulate it using Pandas and finally create an interactive great looking dashboard in Tableau. 

## Metadata
- "timestamp" - timestamp field for grouping the data
- "cnt" - the count of a new bike shares
- "t1" - real temperature in C
- "t2" - temperature in C "feels like"
- "hum" - humidity in percentage
- "wind_speed" - wind speed in km/h
- "weather_code" - category of the weather
- "is_holiday" - boolean field - 1 holiday / 0 non holiday
- "is_weekend" - boolean field - 1 if the day is weekend
- "season" - category field meteorological seasons: 0-spring ; 1-summer; 2-fall; 3-winter.
- "weather_code" category description:
    - 1 = Clear ; mostly clear but have some values with haze/fog/patches of fog in vicinity 
    - 2 = scattered clouds/few clouds 
    - 3 = Broken clouds 
    - 4 = Cloudy 
    - 7 = Rain/light Rain shower/Light rain 
    - 10 = rain with thunderstorm 
    - 26 = snowfall 
    - 94 = Freezing Fog

[Link to Kaggle Dataset](https://www.kaggle.com/datasets/hmavrodiev/london-bike-sharing-dataset/data)

## Data Manipulation using Pandas
**1. Importing necessary libraries**
```python
#!pip install pandas
#!pip install zipfile
#!pip install kaggle

# import pandas library
import pandas as pd

# import zipfile library (To extract the file downloaded from Kaggle)
import zipfile

# import kaggle library (To download the dataset programatically from Kaggle)
import kaggle
```

**2. Download dataset from Kaggle using the Kaggle API**
```python
# To download dataset from kaggle using the Kaggle API
!kaggle datasets download -d hmavrodiev/london-bike-sharing-dataset
```

**3. Extract the file from the downloaded zip file**
```python
zipfile_name = 'london-bike-sharing-dataset.zip'
with zipfile.ZipFile(zipfile_name, 'r') as file:
    file.extractall()
```

**4. Read the CSV file as Pandas Dataframe**
```python
bikes = pd.read_csv("london_merged.csv")
```

**5. Modifying the column names**
```python
new_cols_dict ={
    'timestamp':'time',
    'cnt':'count', 
    't1':'temp_real_C',
    't2':'temp_feels_like_C',
    'hum':'humidity_percent',
    'wind_speed':'wind_speed_kph',
    'weather_code':'weather',
    'is_holiday':'is_holiday',
    'is_weekend':'is_weekend',
    'season':'season'
}

# Renaming the columns to the specified column names
bikes.rename(new_cols_dict, axis=1, inplace=True)
```

**6. Changing the humidity values to percentage (i.e. value between 0 and 1)**
```python
bikes.humidity_percent = bikes.humidity_percent / 100
```

**7. Creating Season and Weather dictionary to map the integers in the column with the actual written values**
```python
# creating a season dictionary so that we can map the integers 0-3 to the actual written values
season_dict = {
    '0.0':'spring',
    '1.0':'summer',
    '2.0':'autumn',
    '3.0':'winter'
}

# creating a weather dictionary so that we can map the integers to the actual written values
weather_dict = {
    '1.0':'Clear',
    '2.0':'Scattered clouds',
    '3.0':'Broken clouds',
    '4.0':'Cloudy',
    '7.0':'Rain',
    '10.0':'Rain with thunderstorm',
    '26.0':'Snowfall'
}

# changing the seasons column data type to string
bikes.season = bikes.season.astype('str')
# mapping the values 0-3 to the actual written seasons
bikes.season = bikes.season.map(season_dict)

# changing the weather column data type to string
bikes.weather = bikes.weather.astype('str')
# mapping the values to the actual written weathers
bikes.weather = bikes.weather.map(weather_dict)
```

**8. Exporting the final Dataframe for visualisation in Tableau**
```python
bikes.to_excel('london_bikes_final.xlsx', sheet_name='Data')
```

## Tableau Visualization
**Visualizations required:**
- Total Bike Rides
- Moving Average (with Set actions, User-defined parameters, Timeline filter)
- Temperature vs Wind speed Heatmap

**Tooltip Charts:**
- Total Rides by Weather
- Total Rides by Hour

**Parameters:**
- Moving Average Duration (Data type - Integer)
- Moving Average Period (Data type - String; Value - day, week, month)

**Other fields used:**
- Min Month
- Max Month
- Moving Average Period Set
- Reference Band
- Temperature Bins (for Heatmap)
- Wind Bins (for Heatmap)

## Dashboard - [Click Here for Live Dashboard](https://public.tableau.com/app/profile/akarshgm/viz/LondonBikeRides-MovingAverageandHeatmap_17318538006630/Dashboard1)

![Tableau Dashboard](https://github.com/AkarshGM/Python-Tableau-London-Bike-Sharing-Project/blob/main/Tableau%20Dashboard.png)

## Tooltip Charts

![Tooltip Charts](https://github.com/AkarshGM/Python-Tableau-London-Bike-Sharing-Project/blob/main/Tooltip%20Charts.png)

[Download the Tableau File](https://github.com/AkarshGM/Python-Tableau-London-Bike-Sharing-Project/blob/main/Tableau%20-%20London%20Bike%20Rides%20-%20Moving%20Average%20and%20Heatmap.twbx)





