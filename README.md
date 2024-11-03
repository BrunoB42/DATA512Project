# DATA512Project

## Introduction

In this project, a thorough analysis of the impact of wildfire smoke on the city of Shreveport, Louisiana will be performed.

This analysis will be comprised of multiple parts.

In the first part of this analysis, contained within `Part1CommonAnalysis.ipynb`, an estimated metric of smoke impact on the city of Shreveport will be devised and a time series model will be developed to forecast this metric into the next 25 years. This smoke estimate will be compared alongside an estimate of daily and yearly AQI in Shreveport in order to both validate the effectiveness of estimated smoke impact and to highlight the ways in which AQI fails as a measurement of wildfire smoke pollution.

In the second part of this analysis, *TO BE DETERMINED*

## Data Sourcing

#### EPA AQS API

In this analysis, data from the [United States Environmental Protection Agency's publicly available AQS API](https://aqs.epa.gov/aqsweb/documents/data_api.html) was obtained in order to estimate the daily and yearly air quality index, or AQI, for Shreveport, LA. In order to request information on the active monitoring stations in the area, the Federal Information Processing Series, or FIPS, code for the region is required. In addition to this, an API key is required in order to be able to make requests. The documentation details how to request an API key.

Data from the Caddo parish that Shreveport is located in was obtained for the last 60 years (1964 - 2024) from three different active monitoring stations in the region. (Note: Louisiana is divided into parishes, not counties like other U.S. states)

Despite the fact that 60 years of data were requested, only 39 years of data were used to obtain AQI estimates in Part 1, as Shreveport did not have particulate data before 1985.

#### Wildland Fire Dataset

The [U.S. Geological Survey's Wildland Fire dataset](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81) was used to generate estimates of smoke impacts on Shreveport from 1964 to 2021. This dataset comes in multiple formats but the cleaned, "combined" layer of its geodatabase file was the only one used for this analysis as it contains fully curated data for both wildfires and prescribed controlled burns. Both are relevant to the estimation of smoke impacts. While this dataset contains data for the entire U.S., only the area surrounding Shreveport was used in the analysis.

The citation for the initial publication of this dataset is as follows:

Welty, J.L., and Jeffries, M.I., 2021, Combined wildland fire datasets for the United States and certain territories, 1800s-Present: U.S. Geological Survey data release, https://doi.org/10.5066/P9ZXGFY3.

NOTE: `Part1CommonAnalysis.ipynb` will expect this data to be placed in a folder titled `wildfire_data`

## Credit And Attribution

Code from Dr. McDonald's `epa_air_quality_history_example.ipynb` notebook was used verbatim in order to query the AQS API for air quality data in section 1 of `Part1CommonAnalysis.ipynb`. This code is used under CC-BY and is the sole work of Dr. McDonald.

The functions `get_first_dates`, `get_last_dates`, and `overlap_with_fire_season` used in section 2 of `Part2CommonAnalysis` are attributed to Himanshu Naidu in the MSDS program and used with permission under their MIT licence.

## Generated Files

Currently none