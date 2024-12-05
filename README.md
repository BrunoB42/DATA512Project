# Impacts of Wildfire Smoke on Asthma in Shreveport, Louisiana

## Introduction

The city of Shreveport is the major industrial center that connects the economies of Arkansas, Louisiana, and Texas. Located in Louisiana’s Caddo parish, the city is home to a complex mix of oil, steel, and healthcare industries all supported by its strategic location at the center of the Ark-La-Tex tri-state area1.

However, the city’s location and industries have also brought with them many long-term issues that Shreveport grapples with to this day. The fumes emitted by the city’s persistent oil refinement industry and constantly expanding steel industry have made respiratory conditions like asthma a constant concern in the area2. Though the thriving healthcare industry has managed to accommodate these needs in the past, recent trends show that a new threat to the city has arrived that threatens to exacerbate this already concerning situation.

Over the past 40 years, wildfires have become exponentially more common all across America. And as the city situated is in the lowest, flattest, and most unobstructed terrain of its area, Shreveport has been seen as experiencing a particularly intense rise in wildfire trends3. In addition to the threat posed by wildfires themselves, the smoke pollution produced by wildfires poses a unique risk to Shreveport’s already smoke-sensitive inhabitants. Wildfire smoke contains a mixture of toxic gasses and particulate matter that severely irritates the respiratory system and can lead to fatal complications for individuals with preexisting respiratory conditions4. In a city so rife with industrial pollution already, the impact of this smoke will be felt most by the vulnerable groups in the community that lack the means by which to mitigate or prevent exposure to these toxic emissions. Those lacking medical insurance, air filtration systems, or housing will have no choice but to endure these conditions until they are ultimately hospitalized.

Though municipal funds in Shreveport are stretched tight and closely regulated, there is still room for the city to shift the balance of its finances in response to the appearance of these destructive wildfires. And this analysis will attempt to answer the question of how asthma hospitalizations are expected to change as wildfire rates continue to climb, and to make a concrete recommendation on how the city should allocate its funds to match this evolving situation.

In this project, a thorough analysis of the impact of wildfire smoke on the city of Shreveport, Louisiana will be performed with the aim of answering the following two questions of interest:

1)	How closely connected are wildfire smoke and air quality in Shreveport? How Well can one predict the other?

2)	Using smoke impacts and historical asthma data as a basis for prediction, how are asthma hospitalization rates expected to change in the coming years for Shreveport?

In order to answer these questions, this analysis will forecast the future impact of smoke on the city of Shreveport and use this forecast to predict the future rate of asthma hospitalizations in the region. This analysis will be accomplished in four steps. 

First, a local air quality dataset will be aggregated to obtain the daily and yearly average values for peak AQI across all wildfire pollutants. Second, a simple model of smoke propagation will be used to estimate the annual impact of fire smoke on Shreveport using a USGS wildfire dataset. Third, the two new datasets will be used to forecast future smoke impacts on Shreveport using an ARIMA time series model. And finally, the smoke impact metrics and historical asthma data will used to predict future annual rates of asthma hospitalization.

The complete project report can be found at the file [ShreveportSmokeAsthmaReport.docx](ShreveportSmokeAsthmaReport.docx)

The analysis notebook can be found at [SmokeAsthmaAnalysis.ipynb](SmokeAsthmaAnalysis.ipynb)

**Note:** The notebook will require the user to provide a valid AQS API key before being able to fully run. The notebook details the steps necessary to obtain this. Running the notebook is expected to take approximately 10 minutes.

## Data Sourcing

#### LDH Asthma Hospitalization Dataset

In the `data_sources` folder, the `caddo_asthma_outcomes.csv` file contains the annual asthma hospitalization data sourced from the Louisiana Department of Health's [Health Data Explorer](https://healthdata.ldh.la.gov/). This data is used in modelling to predict future asthma hospitalization rates in Shreveport after accounting for forecasted smoke impacts.

The two attributes of the asthma hospitalization data used in this analysis are:

- `Year`: Numeric value denoting the year of the associated asthma rate measurement
- `Asthma_Rate`: Numeric value denoting the number of asthma hospitalizations per 10,000 people in Caddo parish

#### EPA AQS API

In the `SmokeAsthmaAnalysis.ipynb` notebook, data from the [United States Environmental Protection Agency's publicly available AQS API](https://aqs.epa.gov/aqsweb/documents/data_api.html) is obtained in order to estimate the daily and yearly air quality index, or AQI, for Shreveport, LA. In order to request information on the active monitoring stations in the area, the Federal Information Processing Series, or FIPS, code for the region is required. The FIPS code for Shreveport’s parish is provided in the notebook. In addition to this, an API key is required in order to be able to make requests. The above AQS API documentation details how to request an API key.

Seven of the air quality data attributes were used in this analysis:

- `site_number`: Unique numeric ID for the location with the air quality monitoring sensor.
- `parameter_code`: 5-digit code identifying the air pollutant being measured.
- `poc`: ID used to distinguish between two different sensors at the same location measuring the same pollutant.
- `latitude`: Latitude of the measuring site
- `longitude`: Longitude of the measuring site
- `date_local`: Local date at time of measurement
- `aqi`: Measured AQI value for the specified pollutant

Data from the Caddo parish that Shreveport is located in was obtained for the fire season (5/1 – 10/31) for last 60 years (1964 - 2024) from four different active monitoring stations in the region. (Note: Louisiana is divided into parishes, not counties). Despite the fact that 60 years of data were requested, only 41 years of data were used to obtain AQI estimates in Part 1, as Shreveport did not have particulate data before 1983.

#### Wildland Fire Dataset

This analysis utilizes wildfire data obtained from the U.S. Geological Survey’s wildland fire dataset. This data is a large-scale geospatial dataset that combines 40 disparate fire datasets in order to describe the type, time, area, and location of fires in the United from the 1850s to 2021. It is provided  through the USGS ScienceBase Catalog under the title Combined wildland fire datasets for the United States and certain territories, 1800s-Present (combined wildland fire polygons). This dataset comes in multiple formats but the cleaned, "combined" layer of its geodatabase file was the only one used for this analysis as it contains fully curated data for both wildfires and prescribed controlled burns. Both are relevant to the estimation of smoke impacts. 

While this dataset contains long-term data for the entire U.S., only data from the past 60 years (1964-2021) within 650 miles of Shreveport that occurred during the fire season (5/1 – 10/31) were utilized in this analysis. The filtering of the dataset by these three constraints is performed within the analysis notebook and does not need to be performed ahead of time.

Six of the wildfire data attributes were used in this analysis:

- `USGS_Assigned_ID`: The unique numeric for each fire. Used as the key in the dataset.
- `Assigned_Fire_Type`: A string containing the type of fire assigned to each entry. String values lie on an five-value ordinal scale varying between `"Prescribed Fire"` and `"Wildfire"` describing the degree to which the fire is likely to be uncontrolled. Used to model hazard level of a fire.
- `Fire_Year`: The year in which the fire occured. Used to aggregate fire data entries to the correct year.
- `Listed_Fire_Dates`: An unstructured list of labelled dates describing the time at which various aspects of the fire occured such as the Ignition Date or Prescribed Fire Start Date as applicable for each fire. Used to calculate the most likely start and end dates for each fire entry.
- `Shape_Area`: The geospatial data entry describing the area of the fire. Used to calcuate smoke output based on fire size.
- The coordinate value innately associated with each geospatial fire entry: Used to calculate the distnce between each fire and Shreveport.

This dataset is made freely available to the public in its initial release, which is cited as:

Welty, J.L., and Jeffries, M.I., 2021, Combined wildland fire datasets for the United States and certain territories, 1800s-Present: U.S. Geological Survey data release, https://doi.org/10.5066/P9ZXGFY3.

**Note:** `SmokeAsthmaAnalysis.ipynb` will expect this data to be placed in the folder `data_sources/wildfire_data` in gdb format, as it is too large to be reasonably stored in the GitHub repository.

#### Generated Files

The `SmokeAsthmaAnalysis.ipynb` notebook does not generate any additional files either as outputs or as intermediates. All intermediate datasets and visualizations are generated and displayed within the jupyter notebook environment.

## Special Credit And Attribution

Code from Dr. McDonald's `epa_air_quality_history_example.ipynb` notebook was used verbatim in order to query the AQS API for air quality data in section 2.2 of `SmokeAsthmaAnalysis.ipynb`. This code is used under CC-BY and is the sole work of Dr. McDonald.

The functions `get_first_dates`, `get_last_dates`, and `overlap_with_fire_season` used in section 2.3 of `SmokeAsthmaAnalysis.ipynb` are attributed to Himanshu Naidu in the MSDS program and used with permission under their MIT licence.

## Known Issues and Limitations

#### Asthma Data Limitations

When attempting to find usable data for modelling asthma hospitalizations in the area, this analysis ran into the complication that asthma hospitalizations are poorly documented compared to mortality.

As hospitalization is likely to have significant physical and financial implications for anyone following an asthma attack, it was important to this analysis that this data be the focus of the project’s modelling. Asthma mortality data fails to capture the true extent of the burden that poor air quality imposes upon people living with asthma and focuses instead on the extremes at which life-threatening complications can occur. Overall, it would be a poor metric for understanding the real human impact of air quality on the citizens of Shreveport.

However, the only available source for asthma hospitalizations in the area that did not group it under a blanket category of “respiratory illnesses” that included COVID-19 was the in-progress dataset published by the Louisiana Department of Health. This dataset has the significant issue of only covering yearly asthma hospitalization rates and only from the years 2000 – 2020. In addition to this, the data from 2016-2020 is openly stated to be poorly characterized and subject to greater uncertainty both due to the advent of COVID-19 and due to incomplete data collection. As such, the predictive modelling was performed with only the well-characterized data from 2000-2015. This had the potential to limit the forecasting capability of the model significantly, though the direction of its predictions still largely agreed with more recent findings.

#### Smoke Impact Limitations

The most relevant issue in this analysis was the implementation of the smoke impact metric. Due to the explicit directions that this analysis received, the spread of wildfire smoke onto Shreveport was estimated directly from the USGS wildland dataset without taking the necessary time to develop a meaningfully accurate model of smoke propagation. The proposed passive diffusion model of smoke impacts is flawed in two fundamental ways that undermine its ability to predict long-term trends in air quality.

The first issue that the model fails to account for is the geography of the region. The city of Shreveport has a large mountain range directly to the west of it that is nearly guaranteed to dampen the flow of air toward the city from the west. While Shreveport itself may be situated in a relatively flat and unobstructed area, the terrain more than 30 miles away from it is characterized by several mountains and hills that affect air flow in complex and relevant ways. Not accounting for this makes it impossible to factor the direction of fire relative to Shreveport into the smoke impact analysis.

The second issue is that the model does not account for the direction of the wind when calculating the spread of smoke from wildfires. The particulate matter within smoke is so well aerosolized that it can follow the path of the wind across several states and affect cities vast distances away from them. It can also be blown away from cities that are directly adjacent to the fires, making proximity a dubious way to infer that smoke from a wildfire will affect a city. Not factoring wind direction into the model of smoke propagation makes it completely unusable as a serious measurement of smoke’s impact on any given location. It is only due to Shreveport’s relatively mild weather that this estimate is able to achieve a resemblance to the area’s AQI. 

## Conclusions

Over the past 40 years, wildfires have become exponentially more common all across America. And this trend has been felt particularly strongly in Ark-La-Tex’s industrial center: Shreveport, Louisiana. Given the city’s history of high asthma hospitalization due to is oil and steel manufacturing industries, there is a concern that an increase in wildfires will lead to an increase in the asthma hospitalization rates that had only just begun to lower in Shreveport.

In order to help guide the decision making of Shreveport’s hall in the face of this sudden issue, this analysis set out to predict the effect that wildfire smoke would have on asthma hospitalizations in Shreveport. In order to accomplish this, wildfire data from the U.S. Geological survey was used to estimate the impact of smoke from every recorded wildfire near Shreveport on the city. In order to forecast smoke impact out into Shreveport’s future, AQI data was obtained from the EPA’s AQS API and used to train a time series forecast model for future smoke impacts. Using this forecast and asthma hospitalization data from the Louisiana Health Department, a predictive model of asthma hospitalizations in Shreveport was developed.

According to both the predictive model of asthma and recent research performed in Shreveport, asthma hospitalization rates are expected to drastically decrease despite increasing wildfires in part due to reduced pollution form the local steel industry. This decline in local pollution is predicted to exceed the effects of smoke for the next 20 years. As such, there is no pressing need to allocate further funds towards asthma.

However, the incidence of wildfires is increasing at an exponential pace. In order to prevent wildfires from becoming an uncontrollable threat to Shreveport’s populace and economy in the far future, this analysis recommends that municipal funds be allocated towards the research and regulation of the factors that enable wildfires to occur so frequently and spread so rapidly in the area surrounding Shreveport, Louisiana. 
