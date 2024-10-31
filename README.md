
# README: Wildfire Smoke Impact Analysis for Peoria, AZ

## Overview

This project aims to estimate and analyze the impact of wildfire smoke on Peoria, AZ, using historical wildfire data from the USGS and air quality data (AQI) from the EPA. The project develops a smoke impact estimate based on proximity to fires, fire size, and seasonal timing. Additionally, we compare this impact estimate to the AQI values to assess any correlation between wildfire activity and local air quality. Finally, the project includes predictive modeling of future smoke impacts over the next 25 years, providing a basis for anticipating and addressing potential future effects.

## Repository Structure

```bash
.
├── LICENSE                                      # Project license file
├── README.md                                    # This README file
├── common_analysis.ipynb                        # Main analysis notebook
├── data                                         # Folder containing datasets
│   ├── USGS_Wildland_Fire_Combined_Dataset.json # Full wildfire dataset
│   ├── Wildfire_short_sample_2024.json          # Sample wildfire data
│   ├── fire_season_gaseous_aqi_peoria.json      # Gaseous AQI data for fire season
│   ├── fire_season_particulate_aqi_peoria.json  # Particulate AQI data for fire season
│   ├── monitoring_stations_peoria.json          # Monitors near Peoria
│   ├── smoke_impact_peoria.csv                  # Annual smoke impact estimates for Peoria
│   └── peoria_wildfire_proximity.json           # Pre-computed wildfire proximity data
├── epa_air_quality_history_example.ipynb        # EPA AQI data retrieval example
├── test.ipynb                                   # Additional tests and validations
├── wildfire                                     # Wildfire module
│   ├── Reader.py                                # Data reading script
│   ├── __init__.py                              # Init file for wildfire module
│   └── __pycache__                              # Cache directory
│       ├── Reader.cpython-39.pyc
│       └── __init__.cpython-39.pyc
└── wildfire_geo_proximity_example.ipynb         # Wildfire proximity computation example
```
  
# Data Directory (`data/`)

This directory contains all input and generated data files used in this analysis:

- **`USGS_Wildland_Fire_Combined_Dataset.json`**:
  - **`attributes`**: Dictionary containing detailed information about each wildfire event.
    - **`OBJECTID`**: Unique identifier for each fire entry.
    - **`USGS_Assigned_ID`**: Assigned ID by USGS for internal tracking.
    - **`Assigned_Fire_Type`**: Classification of fire type (e.g., Wildfire).
    - **`Fire_Year`**: Year in which the fire occurred.
    - **`GIS_Acres`**: Fire area in acres.
    - **`GIS_Hectares`**: Fire area in hectares.
    - **`Source_Datasets`**: Origin dataset(s) from which fire information was obtained.
    - **`Listed_Fire_Names`**: Names associated with the fire in records.
    - **`Listed_Fire_Codes`**: Codes associated with the fire, if any.
    - **`Listed_Fire_IDs`**: Unique identifiers for the fire in external systems.
    - **`Fire_Polygon_Tier`**: Polygon tier level indicating data quality.
    - **`Listed_Fire_Dates`**: Recorded dates for the fire.
    - **`Wildfire_Notice`**: Additional metadata or notices about data accuracy or limitations.

- **`Wildfire_short_sample_2024.json`**: A subset of `USGS_Wildland_Fire_Combined_Dataset.json` containing fewer wildfire events for testing and validation purposes. Variables follow the same structure as the complete wildfire dataset.

- **`fire_season_gaseous_aqi_peoria.json`**: AQI data specific to gaseous pollutants in Peoria during the defined fire season.
  - **`year`**: Year of data recording.
  - **`site_id`**: ID of the monitoring station.
  - **`local_site_name`**: Name of the monitoring site.
  - **`parameter_name`**: Specific gaseous pollutant being measured (e.g., ozone).
  - **`sample_duration`**: Duration of sampling for each AQI reading.
  - **`arithmetic_mean`**: Mean AQI value for the specified duration.

- **`fire_season_particulate_aqi_peoria.json`**: AQI data specific to particulate pollutants in Peoria during the fire season.
  - **`year`**: Year of data collection.
  - **`site_id`**: Monitoring station ID.
  - **`local_site_name`**: Name of the monitoring site.
  - **`parameter_name`**: Type of particulate matter measured (e.g., PM2.5).
  - **`sample_duration`**: Sample collection duration.
  - **`arithmetic_mean`**: Average AQI measurement for the sample duration.

- **`monitoring_stations_peoria.json`**: Contains metadata for monitoring stations in Peoria, AZ.
  - **`station_id`**: Unique identifier for the monitoring station.
  - **`location`**: Coordinates of the monitoring station.
  - **`pollutants_monitored`**: List of pollutants monitored at the station.

- **`peoria_wildfire_proximity.json`**: Data containing proximity calculations between Peoria and various wildfire events.
  - **`fire_id`**: Unique identifier of the fire.
  - **`distance`**: Computed distance from Peoria to the wildfire perimeter.
  - **`year`**: Year the fire occurred.

- **`smoke_impact_peoria.csv`**: Annual smoke impact estimates for Peoria based on fire proximity, fire size, and seasonal occurrence of wildfires within 650 miles.
  - **`Year`**: Year of analysis.
  - **`Smoke Impact`**: Calculated smoke impact estimate for each year, representing an index of smoke-related health and environmental impact based on nearby fires. 

## Key Scripts and Notebooks

- **`common_analysis.ipynb`**: This notebook includes the entire analysis workflow:
  - Computes wildfire proximity and smoke impact.
  - Integrates AQI data and visualizes the comparison.
  - Builds predictive models for future smoke impact forecasts.
  
- **`wildfire_geo_proximity_example.ipynb`**: Contains example code for geodetic calculations to estimate wildfire proximity, designed as a supplement for users to adapt for their specific wildfire data tasks. (David W. McDonald's example modified by me)

- **`epa_air_quality_history_example.ipynb`**: Demonstrates API calls for historical AQI data and provides a model for EPA data retrieval. (David W. McDonald's example modified by me)

## Analysis Steps

### 1. Proximity and Smoke Impact Calculation
   - **Objective**: Calculate an annual wildfire smoke impact for Peoria, AZ.
   - **Method**: Fires within 650 miles of Peoria are filtered by year and only those occurring during the fire season (May 1st to October 31st) are included. The smoke impact is calculated based on fire size and proximity, where closer, larger fires contribute a higher impact score.
   - **Example Attribution**: Distance calculations were based on methods provided in Dr. David W. McDonald’s *Wildfire Proximity Computation Example* notebook.

### 2. AQI Data Retrieval and Processing
   - **Objective**: Obtain particulate and gaseous AQI values for Peoria to compare with wildfire smoke estimates.
   - **Method**: Using the EPA AQS API, AQI data for relevant pollutants are gathered and filtered for the fire season months. Average annual AQI values are calculated to create a comparable metric to the wildfire smoke impact estimates.
   - **Example Attribution**: API requests and data handling were adapted from Dr. McDonald’s *US EPA Air Quality System API Example*, which provided essential functions and API request structures.

### 3. Visualizations
1. **Histogram of Wildfire Distances**: Displays fire frequency at various distance intervals from Peoria.
2. **Annual Total Acres Burned**: Time series of yearly total burned acres for fires within 650 miles.
3. **Comparison of Smoke Impact and AQI**: Time series plot contrasting estimated smoke impact and measured AQI in Peoria.
4. **Forecasted Smoke Impact**: Projection of smoke impact for the years 2025-2050.

### 4. Predictive Modeling
   - **Objective**: Predict future smoke impacts (2025–2050) based on historical trends.
   - **Method**: A linear regression model trained on historical smoke impact data provides predictions, with visualization highlighting projected impacts and trends.

## Instructions

1. **Clone the Repository**:
   To clone the repository:

    ```bash
    git clone https://github.com/apeled/data-512-wildfire-smoke-impact.git
    cd data-512-wildfire-smoke-impact
    ```

2. **Setup**:
   - Install required Python libraries: `pandas`, `numpy`, `matplotlib`, `pyproj`, and `requests`.
   - Obtain and configure an EPA AQS API key in the relevant sections of `epa_air_quality_analysis.ipynb` for AQI data retrieval.

3. **Execution**:
   - Run `wildfire_proximity_analysis.ipynb` to calculate annual smoke impacts.
   - Use `epa_air_quality_analysis.ipynb` to retrieve and process AQI data.
   - Execute `wildfire_predictive_model.ipynb` to train the predictive model and forecast future impacts.

## Known Issues and Limitations

1. **Data Gaps**: Early AQI records are unavailable for some years, impacting the completeness of long-term comparisons.
2. **Simplified Impact Formula**: The smoke impact calculation assumes a basic distance-based inverse relationship, which may not fully capture wind direction or fire intensity.
3. **Predictive Model Simplicity**: The linear model assumes that historical trends continue in a linear fashion, which may not fully represent real-world complexity.

## License and Data Attribution

- This code is licensed under the MIT License.
- **Wildfire data**: Provided by the [USGS ScienceBase repository](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81).
- **EPA AQI data**: Accessed through the [EPA AQS API](https://aqs.epa.gov/aqsweb/documents/data_api.html), used in compliance with the [EPA’s Terms of Use](https://www.epa.gov/privacy/privacy-act-system-records-notices-aqs-air-quality-system).
- **Code Examples**: Based on examples provided by Dr. David W. McDonald’s *Wildfire Proximity Computation Example* and *US EPA Air Quality System API Example*, developed for DATA 512 at UW, under the [Creative Commons CC-BY license](https://creativecommons.org/licenses/by/4.0/).
- This project was developed with the assistance of [ChatGPT](https://openai.com/chatgpt), which helped format code, structure the README, and organize the data and analysis in an efficient and clear manner. All content has been verified and tested by the author.
