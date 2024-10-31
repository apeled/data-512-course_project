
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
│   ├── gaseous_aqi_peoria.json                  # All-season gaseous AQI data
│   ├── monitoring_stations_peoria.json          # Monitors near Peoria
│   ├── particulate_aqi_peoria.json              # All-season particulate AQI data
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

## Data Sources

1. **USGS Wildland Fire Dataset** (`USGS_Wildland_Fire_Combined_Dataset.json`):
   - This dataset contains information on historical wildfires, including fire perimeters, location, and size. The dataset is retrieved from the [USGS ScienceBase repository](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81). Relevant fields used in the analysis include:
     - `Fire_Year`: The year of the fire.
     - `GIS_Acres`: The estimated area burned by the fire.
     - `geometry`: Contains the coordinates defining the perimeter of each fire.

2. **EPA AQI Data** (`particulate_aqi_peoria.json`, `gaseous_aqi_peoria.json`):
   - Air quality data, including particulate and gaseous AQI values, is retrieved using the EPA’s AQS API. The data focuses on daily AQI measures related to PM2.5, PM10, CO, NO2, and O3. Key fields used include:
     - `date`: The date of the AQI observation.
     - `aqi`: The Air Quality Index value for each pollutant.
     - `sample_duration`: Specifies if the reading is based on a 24-hour or hourly sample.

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
    git clone https://github.com/your-username/data-512-wildfire-smoke-impact.git
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
