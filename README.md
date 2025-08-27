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
### Data Analysis
1. What is the relationship between Alcohol consumption and Life expectancy across countries?
   - Descriptive statistics
     ```R
     summary(sample_data$AlcoholConsumption)
     summary(sample_data$LifeExpectancy)
     ```
   - Scatter plot with a regression line
     ```R
     sample_data %>% 
         ggplot(aes(AlcoholConsumption, LifeExpectancy))+
         geom_point(aes(color = Region))+
         geom_smooth(method = "lm", se = T, color = "blue")+
         labs(title = "Alcohol Consumption Vs Life expectancy",
              x = "Alcohol Cunsumption (Litres per capita)",
              y = "Life expectancy (years)")+
         theme_minimal()
     ```
 2. How does the prevalence of Polio or Hepatisi B vaccination influence life expectancy?
    - correlation between vaccinations and Life expectancy
      ```R
      cor(sample_data[, c("Polio", "HepatitisB",
                    "LifeExpectancy")])
      ```
    - Scatter plot with a regression line (Polio and Life Expectancy)
      ```R
      sample_data %>% 
         ggplot(aes(Polio, LifeExpectancy))+
         geom_point(aes(color = Region))+
         geom_smooth(method = "lm", se = T, color = "blue")+
         labs(title = "Polio vaccine Rate Vs Life Expectancy",
              x = "Polio Vaccine (%)",
              y = "Life Expectancy (years)")+
         theme_minimal()
      ```
    - Scatter plot with a regression line (Hepatitis_B and Life Expectancy)
       ```R
       sample_data %>% 
        ggplot(aes(HepatitisB, LifeExpectancy))+
        geom_point(aes(color = Region))+
        geom_smooth(method = "lm", se = T, color = "red")
        ```
 3. To what extent does adult mortality contribute to the variations in life expectancy by Region?
    - Descriptive statistics
      ```R
      summary(sample_data$AdultMortality)
      ```
    - Scatter plot
      ```R
      sample_data %>% 
         ggplot(aes(AdultMortality, LifeExpectancy))+
         geom_point(aes(color = Region))+
         geom_smooth(method = "lm", se = F, color = "blue")+
         labs(title = "Adult Mortality Vs Life expectancy",
              x = "Adult Mortality (per 1000)",
              y = "Life expectancy (years)")+
         theme_minimal()
      ```
    - correlation analysis
      ```R
      cor(sample_data$AdultMortality, sample_data$LifeExpectancy)
      ```
    - Modelling (linear model)
     ```R
      model <- lm(LifeExpectancy ~ AdultMortality + Region, data = sample_data)
      summary(model)
     ```
4. How do BMI and Alcohol consumption together affect life expectancy?
   - Correlation matrix
     ```R
     cor(sample_data[, c("BMI", "AlcoholConsumption",
                    "LifeExpectancy")])
     ```
   - Modelling (Multiple linear moddel)
     ```R
     model2 <- lm(LifeExpectancy ~ BMI + AlcoholConsumption, data = sample_data)
     summary(model2)
     ```
5. What combination of health indicators best predicts life expectancy using multiple regression analysis?
   - selecting health indicators for a new sample data set (healthInc_data)
     ```R
     healthInc_data <- sample_data %>% 
         select(LifeExpectancy, BMI, InfantDeaths, AdultMortality,
                AlcoholConsumption, InfantDeaths, HIVincidence, HepatitisB)
     ```
   - Fitting a multiple linear regression model (model3)
     ```R
     model3 <- lm(LifeExpectancy ~ ., data = healthInc_data)
     summary(model3)
     ```
   - Variable Importance analysis
     ```R
     set.seed(100)
     model_imp <- train(LifeExpectancy ~ ., data = healthInc_data, model = "lm")
     varImp(model_imp)

     par(mfrow = c(2, 2))
     plot(model_imp)
     ```
### Results and Findings
1. What is the relationship between Alcohol consumption and Life expectancy across countries?
   - The scatter plot shows that there is a positive correlation between Alcohol consumption and Life expectancy across different world regions. As alcohol consumption also increases, life expectancy also tends to increase. African countries tend to have lower life expectancy and lower alcohol consumption. European Union and Rest of Europe show high alcohol consumption with relatively high life expectancy. 

     
   

     

   
