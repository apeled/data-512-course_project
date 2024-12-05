
# README: Wildfire Smoke Impact Analysis for Peoria, AZ

## Overview

This project aims to estimate and analyze the impact of wildfire smoke on Peoria, AZ, by combining historical wildfire data from the USGS, air quality data (AQI) from the EPA, and local economic data through stock performance analysis. The initial focus was on developing a smoke impact estimate based on proximity to fires, fire size, and seasonal timing, which was then compared to AQI values to assess the relationship between wildfire activity and local air quality.

Building upon this foundation, the analysis was extended to incorporate economic impacts. Using stock performance data from companies in the Peoria region and the S&P 500 as a benchmark, the project explored how wildfire smoke might affect local economic conditions. Predictive modeling was applied to both smoke impact and adjusted stock performance, providing insights into potential future environmental and economic outcomes over the next 25 years.

By integrating geospatial, environmental, and economic datasets, this project demonstrates the complexities of wildfire impacts on both public health and economic systems, offering a multidisciplinary perspective on the challenges posed by increasing wildfire activity.

## Repository Structure

```bash
.
├── LICENSE                                      # Project license file
├── README.md                                    # This README file
├── common_analysis.ipynb                        # Main analysis notebook for Part 1
├── extension_analysis.ipynb                     # Extension analysis notebook including economic data
├── data                                         # Folder containing datasets
│   ├── USGS_Wildland_Fire_Combined_Dataset.json # Full wildfire dataset (not committed due to size)
│   ├── Wildfire_short_sample_2024.json          # Sample wildfire data for testing/validation
│   ├── fire_season_gaseous_aqi_peoria.json      # Gaseous AQI data for Peoria during fire seasons
│   ├── fire_season_particulate_aqi_peoria.json  # Particulate AQI data for Peoria during fire seasons
│   ├── maricopa_county_stocks_monthly.csv       # Monthly stock data for local companies
│   ├── smoke_impact_peoria.csv                  # Annual smoke impact estimates for Peoria
│   └── sp500_monthly.csv                        # Monthly S&P 500 index data for benchmarking
├── docs                                         # Supporting documents and deliverables
│   ├── DATA_512_Common_Analysis.pdf             # Part 1: Visualizations and Reflection Statement
│   ├── DATA_512_Extension_Plan.pdf              # Project plan for analysis extension
│   └── apeled_presentation_peoria_az.pdf        # Presentation summarizing findings
├── epa_air_quality_history_example.ipynb        # EPA AQI data retrieval example
├── wildfire_geo_proximity_example.ipynb         # Wildfire proximity computation example
└── wildfire                                     # Wildfire module
    ├── Reader.py                                # Data reading script
    ├── __init__.py                              # Init file for wildfire module
    └── __pycache__                              # Cache directory
        ├── Reader.cpython-39.pyc
        └── __init__.cpython-39.pyc

```
  
## Data Directory (`data/`)

This directory contains all input and generated data files used in the analysis. Several large files have been excluded from the repository but can be processed using provided scripts and examples.

### **Available Datasets**

- **`USGS_Wildland_Fire_Combined_Dataset.json`**  
  - **Description**: Full wildfire dataset from the USGS ScienceBase repository.  
  - **Fields**:
    - `OBJECTID`: Unique identifier for each fire entry.
    - `USGS_Assigned_ID`: Assigned ID by USGS for internal tracking.
    - `Assigned_Fire_Type`: Classification of fire type (e.g., Wildfire).
    - `Fire_Year`: Year in which the fire occurred.
    - `GIS_Acres`: Fire area in acres.
    - `GIS_Hectares`: Fire area in hectares.
    - `Source_Datasets`: Origin dataset(s) from which fire information was obtained.
    - `Listed_Fire_Names`: Names associated with the fire in records.
    - `Listed_Fire_Codes`: Codes associated with the fire, if any.
    - `Listed_Fire_IDs`: Unique identifiers for the fire in external systems.
    - `Fire_Polygon_Tier`: Polygon tier level indicating data quality.
    - `Listed_Fire_Dates`: Recorded dates for the fire.
    - `Wildfire_Notice`: Additional metadata or notices about data accuracy or limitations.

- **`Wildfire_short_sample_2024.json`**  
  - **Description**: A subset of the `USGS_Wildland_Fire_Combined_Dataset.json` for testing and validation purposes.  
  - **Fields**: Follows the same structure as the full wildfire dataset.

- **`fire_season_gaseous_aqi_peoria.json`**  
  - **Description**: AQI data specific to gaseous pollutants in Peoria during the defined fire season.  
  - **Fields**:
    - `year`: Year of data recording.
    - `site_id`: Monitoring station ID.
    - `local_site_name`: Name of the monitoring site.
    - `parameter_name`: Specific gaseous pollutant measured (e.g., ozone).
    - `sample_duration`: Sampling duration for AQI readings.
    - `arithmetic_mean`: Mean AQI value for the specified duration.

- **`fire_season_particulate_aqi_peoria.json`**  
  - **Description**: AQI data specific to particulate pollutants in Peoria during the fire season.  
  - **Fields**:
    - `year`: Year of data collection.
    - `site_id`: Monitoring station ID.
    - `local_site_name`: Name of the monitoring site.
    - `parameter_name`: Type of particulate matter measured (e.g., PM2.5).
    - `sample_duration`: Sample collection duration.
    - `arithmetic_mean`: Average AQI measurement for the sample duration.

- **`maricopa_county_stocks_monthly.csv`**  
  - **Description**: Monthly stock data for companies operating in Maricopa County, AZ.  
  - **Fields**:
    - `Ticker`: Stock ticker symbol for each company.
    - `date`: Date of the stock data.
    - `1. open`: Opening stock price.
    - `2. high`: Highest stock price for the day.
    - `3. low`: Lowest stock price for the day.
    - `4. close`: Closing stock price for the day.
    - `5. volume`: Volume of stocks traded.
    - `YearMonth`: Derived field indicating the year and month for aggregation.

- **`sp500_monthly.csv`**  
  - **Description**: Monthly data for the S&P 500 index, used as a benchmark for detrending stock performance.  
  - **Fields**:
    - `Date`: Date of the S&P 500 data.
    - `1. open`: Opening index value.
    - `2. high`: Highest index value for the day.
    - `3. low`: Lowest index value for the day.
    - `SP500_Close`: Closing index value for the day.
    - `5. volume`: Volume of index trades.
    - `YearMonth`: Derived field indicating the year and month for aggregation.

- **`smoke_impact_peoria.csv`**  
  - **Description**: Annual smoke impact estimates for Peoria, AZ, based on wildfire proximity, fire size, and seasonal occurrence.  
  - **Fields**:
    - `Year`: Year of analysis.
    - `Smoke Impact`: Calculated smoke impact score, representing an index of smoke-related health and environmental impact.

### **Excluded Datasets**

Some larger datasets, including `monitoring_stations_peoria.json` and `peoria_wildfire_proximity.json`, are not included in the repository due to size constraints but can be regenerated using scripts provided in the `wildfire_geo_proximity_example.ipynb` notebook.

- **monitoring_stations_peoria.json**: Contains metadata for monitoring stations in Peoria, AZ.
  - `station_id`: Unique identifier for the monitoring station.
  - `location`: Coordinates of the monitoring station.
  - `pollutants_monitored`: List of pollutants monitored at the station.

- **peoria_wildfire_proximity.json**: Data containing proximity calculations between Peoria and various wildfire events.
  - `fire_id`: Unique identifier of the fire.
  - `distance`: Computed distance from Peoria to the wildfire perimeter.
  - `year`: Year the fire occurred.

This directory serves as the central repository for all input and processed data used in the project, supporting both environmental and economic impact analyses.

## Key Scripts and Notebooks

This section outlines the primary scripts and notebooks used for the analysis, covering both the initial wildfire and air quality analysis and the extended economic impact analysis.

### **Primary Notebooks**

- **`common_analysis.ipynb`**  
  - **Description**: Notebook for Part 1 of the analysis.
  - **Features**:
    - Computes wildfire proximity and annual smoke impact scores for Peoria.
    - Integrates AQI data from the EPA and visualizes comparisons with smoke impact estimates.
    - Builds a linear regression model to forecast future smoke impacts (2025–2050).
    - Includes interactive visualizations (e.g., dual-axis plots) to highlight key trends.

- **`extension_analysis.ipynb`**  
  - **Description**: Notebook extending the analysis to include economic data.
  - **Features**:
    - Processes monthly stock data for local companies and S&P 500 index data.
    - Detrends stock performance to isolate local economic factors from broader market trends.
    - Implements machine learning models (Gradient Boosting, LSTM) to predict adjusted stock changes.
    - Visualizes relationships between smoke impact and stock performance, including future projections.

### **Supplementary Notebooks**

- **`wildfire_geo_proximity_example.ipynb`**  
  - **Description**: Code for geospatial calculations of wildfire proximity to Peoria. (David W. McDonald's example modified by me)
  - **Features**:
    - Demonstrates use of geodetic distance calculations (e.g., Pyproj library).
    - Includes distance filtering logic for fires within a defined radius of Peoria.

- **`epa_air_quality_history_example.ipynb`**  
  - **Description**: Notebook for retrieving AQI data using the EPA AQS API. (David W. McDonald's example modified by me)
  - **Features**:
    - Shows how to request and process AQI data by county and pollutant type.
    - Explains handling of pagination and varying API parameters.
    - Provides a template for integrating AQI data into other analyses.

### **Supporting Documents**

- **`docs/DATA_512_Common_Analysis.pdf`**  
  - Deliverable for Part 1, including visualizations and a reflection statement.

- **`docs/DATA_512_Extension_Plan.pdf`**  
  - Extension plan outlining the goals and methodology for incorporating economic data into the analysis.

- **`docs/apeled_presentation_peoria_az.pdf`**  
  - Final presentation summarizing the key findings from both the initial and extended analyses.


These scripts and notebooks form the core of the project, providing a comprehensive framework for analyzing the environmental and economic impacts of wildfire smoke in Peoria, AZ.


## Analysis Steps

This section outlines the key steps taken during the analysis, starting with Part 1, which focused on wildfire and air quality data, and continuing with the extended analysis incorporating economic data.

### **Part 1: Wildfire and Air Quality Analysis**

#### 1. Proximity and Smoke Impact Calculation
   - **Objective**: Calculate an annual wildfire smoke impact score for Peoria, AZ.
   - **Method**: 
     - Filter wildfires within 650 miles of Peoria.
     - Include only fires occurring during the fire season (May 1 to October 31).
     - Calculate smoke impact based on fire size and proximity, where closer, larger fires contribute a higher impact score.
   - **Example Attribution**: Geodetic distance calculations were based on methods provided in Dr. David W. McDonald’s *Wildfire Proximity Computation Example* notebook.

#### 2. AQI Data Retrieval and Processing
   - **Objective**: Obtain particulate and gaseous AQI values for Peoria to compare with wildfire smoke impact estimates.
   - **Method**:
     - Use the EPA AQS API to retrieve AQI data for relevant pollutants during fire season months.
     - Calculate average annual AQI values to create a comparable metric to the wildfire smoke impact estimates.
   - **Example Attribution**: API requests and data handling were adapted from Dr. McDonald’s *US EPA Air Quality System API Example*.

#### 3. Visualizations
   - **Histogram of Wildfire Distances**: Displays the frequency of wildfires at various distance intervals from Peoria.
   - **Annual Total Acres Burned**: A time series plot of yearly total burned acres for fires within 650 miles.
   - **Comparison of Smoke Impact and AQI**: A dual-axis time series plot contrasting estimated smoke impact with measured AQI values.
   - **Forecasted Smoke Impact**: A projection of smoke impact for the years 2025–2050 using a linear regression model.

#### 4. Predictive Modeling
   - **Objective**: Predict future smoke impacts (2025–2050) based on historical trends.
   - **Method**: 
     - Train a linear regression model on historical smoke impact data.
     - Forecast future impacts and visualize the trends to aid in planning and mitigation efforts.

---

### **Part 2: Extended Economic Analysis**

#### 5. Stock Data Retrieval and Processing
   - **Objective**: Incorporate monthly stock data for companies operating in Maricopa County to evaluate the economic impacts of wildfire smoke.
   - **Method**:
     - Retrieve historical stock data using the Alpha Vantage API.
     - Process data for selected companies and the S&P 500 index as a benchmark for market trends.
     - Normalize stock prices and compute percent changes for trend analysis.

#### 6. Detrending Stock Data
   - **Objective**: Remove broader market influences to isolate local economic factors potentially related to wildfire smoke.
   - **Method**:
     - Calculate adjusted stock changes by subtracting S&P 500 percent changes from individual company stock percent changes.
     - Use moving averages to further smooth and analyze trends.

#### 7. Feature Engineering
   - **Objective**: Prepare data for machine learning models by incorporating lagged features and standardizing variables.
   - **Method**:
     - Create lagged features for smoke impact and adjusted stock changes to capture temporal dependencies.
     - Scale features to ensure compatibility with machine learning models.

#### 8. Modeling
   - **Gradient Boosting Regression**:
     - Predict adjusted stock changes based on smoke impact and lagged features.
     - Evaluate model performance using metrics like Mean Squared Error (MSE) and R-squared.
   - **LSTM Neural Network**:
     - Capture sequential dependencies in stock data using Long Short-Term Memory (LSTM) networks.
     - Compare predictions with those from the Gradient Boosting model.

#### 9. Extended Visualizations
   - **Normalized Stock Prices Over Time**: Compare normalized stock prices of local companies with the S&P 500 index.
   - **Detrended Stock Data Over Time**: Visualize adjusted stock changes after removing market trends.
   - **Average Smoke Impact vs. Stock Performance**: A dual-axis plot showing the relationship between annual smoke impact and adjusted stock changes.
   - **Forecasted Impacts**:
     - Predict future smoke impacts and their economic effects using Gradient Boosting and LSTM models.
     - Visualize scenarios from 2025–2050.


## Instructions

Follow these steps to set up and execute the analysis, covering both the initial wildfire and air quality analysis and the extended economic analysis.

### **1. Clone the Repository**

Clone the repository to your local machine:

```bash
git clone https://github.com/apeled/data-512-wildfire-smoke-impact.git
cd data-512-wildfire-smoke-impact
```

### **2. Environment Setup**

Ensure you have Python 3.8 or above installed. Install the required dependencies by running:

```bash
pip install -r requirements.txt
```

The project requires the following libraries:
- `pandas`
- `numpy`
- `matplotlib`
- `pyproj`
- `requests`
- `scikit-learn`
- `tensorflow`
- `alpha_vantage`


### **3. API Keys Setup**

#### EPA AQS API Key
1. Obtain an API key from the [EPA AQS API](https://aqs.epa.gov/aqsweb/documents/data_api.html#signup).
2. Replace the placeholder in `epa_air_quality_history_example.ipynb` with your API key:
   ```python
   api_key = "YOUR_EPA_API_KEY"
   ```

#### Alpha Vantage API Key
1. Register for an account at [Alpha Vantage](https://www.alphavantage.co/).
2. Obtain an API key by navigating to the "GET FREE API KEY" found on their main page.
3. Replace the placeholder in `extension_analysis.ipynb` with your API key:
   ```python
   api_key = "YOUR_ALPHA_VANTAGE_API_KEY"
   ```

### **4. Execute Part 1: Wildfire and Air Quality Analysis**

1. **Run `common_analysis.ipynb`**:
   - Computes wildfire proximity and annual smoke impact scores for Peoria.
   - Integrates AQI data using the EPA AQS API.
   - Visualizes comparisons between wildfire smoke impact and AQI.
   - Forecasts future smoke impacts using a linear regression model.

2. Ensure the necessary datasets are present in the `data/` directory:
   - `USGS_Wildland_Fire_Combined_Dataset.json`
   - `fire_season_gaseous_aqi_peoria.json`
   - `fire_season_particulate_aqi_peoria.json`
   - `smoke_impact_peoria.csv`

### **5. Execute Part 2: Extended Economic Analysis**

1. **Run `extension_analysis.ipynb`**:
   - Processes stock data for local companies and the S&P 500 index.
   - Detrends stock performance to isolate local economic factors.
   - Builds predictive models (Gradient Boosting, LSTM) for adjusted stock changes.
   - Visualizes the relationship between wildfire smoke and stock performance.
   - Forecasts future economic impacts using predictive modeling.

2. Ensure the following datasets are present in the `data/` directory:
   - `maricopa_county_stocks_monthly.csv`
   - `sp500_monthly.csv`
   - `smoke_impact_peoria.csv`


### **6. Reproducing Results**

To reproduce all results:
- Execute `common_analysis.ipynb` to compute smoke impacts and AQI comparisons.
- Execute `extension_analysis.ipynb` for economic data integration and modeling.
- Visualizations and results are saved directly within the notebooks.

### **7. Optional: Regenerate Datasets**

For larger excluded datasets (e.g., proximity calculations), use the provided example notebooks:
- **`wildfire_geo_proximity_example.ipynb`**: Compute wildfire proximities for Peoria using geospatial calculations.
- **`epa_air_quality_history_example.ipynb`**: Retrieve AQI data for Peoria using the EPA AQS API.



## Known Issues and Limitations

### **Wildfire and Air Quality Analysis**

1. **Data Gaps in AQI Records**:
   - Early AQI records are unavailable for some years, particularly for certain pollutants, which impacts the completeness of long-term comparisons with smoke impact estimates. This gap limits the accuracy of trends in the early analysis period and may underrepresent historical wildfire smoke impacts.

2. **Simplified Smoke Impact Formula**:
   - The smoke impact calculation assumes a basic inverse relationship between fire size, proximity, and impact. This approach does not account for external factors like wind patterns, terrain, or fire intensity variations. The fixed 650-mile radius may exclude fires with far-reaching impacts or include smaller, irrelevant fires.

3. **Assumptions in Proximity Calculations**:
   - Geospatial calculations rely on centroid distances between Peoria and wildfire perimeters, which may oversimplify the true spread of smoke and pollutants.
  
### **Extended Economic Analysis**

1. **Stock Data Limitations**:
   - Stock data reflects a mix of local, national, and global economic factors, making it difficult to attribute changes solely to wildfire smoke impacts. Smaller, local companies may show higher sensitivity to environmental disruptions, but such data may not fully capture this due to limited representation.

2. **Simplified Detrending Approach**:
   - Adjusted stock changes are calculated by subtracting S&P 500 percent changes, which may not fully account for broader market volatility or sector-specific influences.

3. **Predictive Model Assumptions**:
   - Both Gradient Boosting and LSTM models assume historical trends in stock performance and smoke impacts will continue into the future, which may not account for unprecedented environmental or economic changes.
   - The models rely on engineered features (e.g., lagged values) that could oversimplify complex temporal relationships.

Despite these limitations, the analysis provides a strong foundation for understanding the intersection of environmental and economic impacts of wildfire smoke in Peoria, AZ. Future extensions could address these issues by incorporating additional datasets, refining models, and expanding the geographical scope of the study.


## License and Data Attribution

- This code is licensed under the MIT License.
- **Wildfire data**: Provided by the [USGS ScienceBase repository](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81).
- **EPA AQI data**: Accessed through the [EPA AQS API](https://aqs.epa.gov/aqsweb/documents/data_api.html#signup), used in compliance with the [EPA’s Terms of Use](https://www.epa.gov/privacy/privacy-act-system-records-notices-aqs-air-quality-system).
- **Stock data**: Retrieved using the [Alpha Vantage API](https://www.alphavantage.co/) under their free-tier license, adhering to the API’s terms of use.
- **Code Examples**: Based on examples provided by Dr. David W. McDonald’s *Wildfire Proximity Computation Example* and *US EPA Air Quality System API Example*, developed for DATA 512 at UW, under the [Creative Commons CC-BY license](https://creativecommons.org/licenses/by/4.0/).
- **Geodetic distance calculations**: Utilized geospatial libraries like Pyproj, with methodology inspired by Dr. McDonald’s example notebooks.
- **Machine Learning Models**: The implementation of Gradient Boosting Regression and LSTM was inspired by various publicly available resources and tailored for this analysis.
- This project was developed with the assistance of [ChatGPT](https://openai.com/chatgpt), which helped format code, structure the README, and organize the data and analysis in an efficient and clear manner. All content has been verified and tested by the author.

