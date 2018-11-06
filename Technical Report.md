# Technical Report - MTA Subway Data Regression

## Problem Statement

The Metropolitain Transportation Authority (MTA) is the public benefit corporation responsible for piblic transportation, including the New York City Subway which connets four boroughs, Brooklyn, Queens, Manhattan and the Bronx.   Each year there are over one and half billion riders on the systems annually ranking it in the top 10 bussiest Subway Systems in the world and the bussiest in North America.   

My goal was to build two models to predict ridership for the Price St station in Manhattan.  The first would only have features engineers from only the data provided from the MTA and the second would allow additional features from the weather data provided by NOAA. I would then compare the and see how much, if any, the model improved. 


## Data Collection 

A large trove of MTA data is available through New York Sates [data.ny.gov portal](https://data.ny.gov/) including ridership data for each turnstile on the system going back to [1Feb2014](https://data.ny.gov/Transportation/Turnstile-Usage-Data-2014/i55r-43gk).  This includes information broken out into time slices which are set to begin daily between 00 to 03 hours and repeat every 4 hours for each indivudal turnstile in the system.  Each row contiain 11 columns of information identiflying the Name of the Station where the turnstile is located, the Unit ID of the station, the control area or booth name and a subuit position idenification for each indivudual device.  Information is also provided as to which train lines stop at the location, weather the line originally belonged to the BMT, IRT or IND. The date, the time and a description if the information was gathered froma  regular scheduled audit event or if it was a recovery audit as well as the cumulative ENTRY and EXIT register values for a given device.  The Data Dictionary can be found here [pdf download](https://data.ny.gov/api/views/i55r-43gk/files/wvX7ZEZpMrzjwBd2r_ZE3Nl4OLdJFP_t32osotBZPi0?download=true&filename=MTA_Turnstile_Data_DataDictionary.pdf)
 
National Oceanic and Athmospheric Administration [NOAA](https://www.noaa.gov/) provides users a [portal](https://www.ncdc.noaa.gov/cdo-web/search) to request weather data.  From this I was able to request daily weather data for the Centrl Park Weather Station approximately 4 miles away from the Prince St subway station.  Weather data used was temperture MIN and MAX in fahrenheit, precipitation in inches and snow in inches.  

Data was obtained in a .csv format.  

## Exploratory Data Analysis

[EDA Notebook for MTA data](https://git.generalassemb.ly/JasonMallet/Prince-St/blob/master/Notebooks/EDA%20MTA%20data.ipynb)

[EDA Notebook for NOAA Weather data](https://git.generalassemb.ly/JasonMallet/Prince-St/blob/master/Notebooks/EDA%20NOAA%20Weather%20data.ipynb)


The combined dataset from Feb 2014 to Sep 2018 contained information for each turnstile for each of the over 400 individual stations int he system in the almost 5 year period. Totally over 46 million rows with the each of the eleven columns described above, this is over 500 million raw points of data.  Systemwide the work week has the highest daily ridership with weekend days having close to half the ridership of a workweek day. 

Systemwide Entrances by Day of the Week

![System Day of Week](https://git.generalassemb.ly/JasonMallet/Prince-St/blob/master/Visuals/A%20MTA%20Subway%20Entrance%20by%20Day%20of%20Week.PNG)


Systemwide Entrances by Date

![System Date](https://git.generalassemb.ly/JasonMallet/Prince-St/blob/master/Visuals/MTA%20Subway%20Entrances%20by%20Date.PNG)

Focusing only only Prince St trims the rows down to only approximately 760,000 or around 1.64% of the total systemwide dataset. There were 15 individual turnstills in use over the observation period of which two were either fully broken or disabled as they showed counts of 0 for the entirety of the appearance in the data set.  While two others had their couters reset during observation period.  This resulted in negative differences when calulating Hourly block instance counts.   Prince St ridership does not follow the same pattern as the rest of the system in that it does not experiance a large drop off for Saturday Which is actually busier than Monday, and while Sunday is still the day with the lowest ridership it is only around a third instead of the half of the whole system.  

Prince St Entrances by Day of the Week
![Prince St Day of Week](https://git.generalassemb.ly/JasonMallet/Prince-St/blob/master/Visuals/A%20Prince%20St%20Subway%20Entrance%20by%20Day%20of%20Week.PNG)

When looking at Daily Weather data from NOAA Precipitation and Snow looked to have more impact on ridership than temperature. 

![Prince St Entrances by Day of week PRCP](https://git.generalassemb.ly/JasonMallet/Prince-St/blob/master/Visuals/Prince%20St%20Day%20of%20Week%20with%20Precip.PNG)


![Prince St Entrances by Day of week SNOW](https://git.generalassemb.ly/JasonMallet/Prince-St/blob/master/Visuals/Prince%20St%20Day%20of%20Week%20with%20Snow.PNG)

## Modeling

[Modeling Notebook](https://git.generalassemb.ly/JasonMallet/Prince-St/blob/master/Notebooks/Prince%20St%20Modeling.ipynb)

All statistical analysis was done on a DELL XPS 15 9570 with Intel(R) Core(TM) i7-8750H CPU running Microsoft Windows 10 Home.

ARIMA and Linder Regresion Models were used to predict Ridership



## Results

Without weather data the model was able to predict Entrances to Prince Street, which has an average of close to 15,000 per day with a RSME on the Test set of 1686 and R2 score of 41.9% 

![Ridge Base](https://git.generalassemb.ly/JasonMallet/Prince-St/blob/master/Visuals/Ridge%20Real%20vs%20Predicted%20base.PNG)

while with weather data included it was a RSME of 1546 with and R2 of 50.6% on the Test

![Ridge Weather](https://git.generalassemb.ly/JasonMallet/Prince-St/blob/master/Visuals/Ridge%20Real%20vs%20Predicted%20weather.PNG)


## Future Steps

**Redo turnstile aggrigation** To more accurately account for daily traffic patters by delimiating days at the 03 or 04 hour Time Stamp period instead of by the date in which the stamp occured.  Many trips are taken beween 11pm and midnight or even between midnight and 4am which are more accurately attributed to the prior days activity than the actual date in which the trip took place.  Currently a aam trip on Sunday morning is being attributed to Sunday while in fact the liklehood of the trip is more attributed to someone going home after a being out late on a Saturday night. This could more accuratly deliniate trips by the correct day of the week leading to imprvoed results.  

**Additional Feature Engineering**  While weather data improved the accuracy of the model, additional engeering of the weather data could further improve the model reducing large outliers.  

**Additional Model Exploration** While ARMA and Linear Regression showed some predictive power, continual exploration of Newural Networks LSTM model could yeild additional insights.


