# Global Life-expectancy-Analysis Project
## Statistical analysis on factors influencing life expectancy

### Project Overview

This data analysis project explores how various health and social indicators impact life expectancy across countries. It also seeks to quantify the most influential indicators to inform evidence-based health planning and resource allocation.

### Data Source

Life Expectancy (WHO):The data used for this analysisis the "Life_Expectancy_data.csv" file, downloaded from (https://www.kaggle.com/datasets/kumarajarshi/life-expectancy-who). The objective is to demostrate dta analysis skills using R.

### Tools

R language
 - library(readr) for loading data
 - library(dplyr) for data manipulation
 - library(ggplot2) for data visualization graphics
 - library(caret) for analyzing feature importance

### Data Cleaning and Preparation
 - Data loading and examination
```R
library(readr)
data <- read_csv("C:\\Users\\Fakudze\\Desktop\\archive (23).zip")
View(data)
glimpse(data)
```
 - Checking for missing values
```R
colSums(is.na(data))
```
 - Renaming some variables
```R
data <- data %>% 
         rename(LifeExpectancy =  Life_expectancy,
                HepatitisB = Hepatitis_B,
                AdultMortality = Adult_mortality,
                InfantDeaths = Infant_deaths,
                Under5deaths = Under_five_deaths,
                AlcoholConsumption = Alcohol_consumption,
                HIVincidence = Incidents_HIV)
```
 - Variable selection
```R
sample_data <- data %>% 
         select(LifeExpectancy, Population_mln, GDP_per_capita,
                HIVincidence, Polio, BMI, HepatitisB,
                AlcoholConsumption, AdultMortality,
                InfantDeaths, Region, Country) 
View(sample_data)
```
