# Filling the Gaps; Bicing Use and How Data Analytics Can Help Institutions Complete Historical Data Records #

## Edward Rodger Martinez - Ironhack Final Project ##

### December 2023 ###

Gif here of predictive model?

Please feel free to explore the following Tableau link for an interactive version of the data visualisations.

https://public.tableau.com/views/FillingtheGapsBicingandDataAnalytics/Model?:language=en-GB&publish=yes&:display_count=n&:origin=viz_share_link

### Overview ###

This is a Data Analytics project using databases from Open Data Barcelona on the use of Bicing public bicycles in Barcelona. The database contains timestamps at intervals of between 5 and 15 minutes, and for each timestamp provides a number of bicycles in use across Barcelona at that exact time. The timespan covered by the available data is from August to December 2018, in 5 separate databases (one per month). When I contacted the Ajuntament de Barcelona (Barcelona City Council) to ask if more data was available, they referred me to a document stating that no further data was available due to a change of system in 2019. 

The goal of this project is to identify patterns in the use levels of Bicing bicycles, and to train a predictive model to predict levels of bicycle use, in order to help the city council complete its incomplete dataset. 

The project therefore addresses a very common problem in the public sector; public institutions are often under tight budgets and have inconsistent methods for collecting data, often leading to incomplete, patchy datasets. More broadly, therefore, this project explores how predictive modelling can help public institutions fill gaps in their historical data records, helping them get better insights and a more complete picture in order to make better decisions going forward.
 

### Requirements and Libraries ###

Code was written on Jupyter Notebook and Visual Studio Code, some queries and EDA were carried out on SQL, and data visualisations were done on Tableau. Additionally, the Open Meteo API was used to add the weather components to the dataset. The following libraries were used within Python: 

pandas
numpy
datetime
seaborn
matplotlib
statsmodels
sklearn
requests_cache
retry_requests
openmeteo_requests

### Workflow ###

Step 1: Concatenated 5 datasets from Open Data Barcelona, showing number of bikes in use at 5-15 minute time intervals for each month between August and December 2018. The resulting concatenated dataset had approximately 26 000 rows.

Step 2: Did some feature engineering to expand the dataset. With the use of the datetime library; I added a column stating if the datetime value in each row corresponded to a weekday or weekend, another column stating if it was a bank holiday (after manually adding a list of Barcelona bank holidays), another column stating if it was the eve of a weekend or bank holiday (eg. Fridays), and another column categorising the time of day into morning (6-12AM), afternoon (12AM-6PM), evening (6-12PM) and night (12PM-6AM). 

Step 3: Used the Open Meteo API to integrate 2 new columns; one showing the level of precipitation at that timestamp in Barcelona (in mm), and the other showing the temperature (in degrees celsius), to the nearest hour. The API provided historical weather data with time intervals of 1 hour, so it was slightly less exact than the data set I had (time intervals between 5-15 minutes). For exmaple; this meant that for timestamp 18:36:00, the weather information in that row corresponds to the API's information for 18:00:00. 

Image here of final data set?

Step 4: Using this transformed data set, I imported the resulting csv file into Tableau and made the visualisations showing daily and weekly seasonality in Bicing use, as well as how use levels correlate with rainfall and temperature. 

Step 5: I used my transfored data set as a base to make a second dataset for the predictive model. Since I planned on using the predictive model to fill the gap in data from January 2019 to November 2023, I added in columns with hourly timestamps showing the same information as in my previous dataset - time of day, weekend/bank holiday vs weekday, rainfall and temperature. All the same steps were repeated. I then did some feature engineering to turn all the categorical columns into dummies. 

Step 6: I trained different predictive models on the available data. The features were; time of day, weekend/bank holiday or weekday, eve of weekend/bank holiday, temperature and rainfall. The target variable was the number of bikes in use. I used a Random Forest Regressor with an R2 of 0.85 and a RMSE of 89. I added a column to my dataset for 'y pred', representing the model's prediction for number of bikes in use at each given timestamp, side by side with the real data. From January 2019 onwards, there was no real data for the number of bikes in use, only the prediction. 

Image here of model data set?

Step 7: Using this dataset, I put together the visualisations to show the model's accuracy, as well as its use and limitations. 

### Daily and Weekly Seasonality in Bicing Use ###

Gif of data + pictures of additional graphs




### Rain and Bicing Use ###

### Temperature and Bicing Use ###

### Training and Implementing a Predictive Model for Number of Bicing Bikes in Use at Any Given Time ###

### Conclusions ###

### Limitations ###

1. Inefficient coding/organisation: due to time constraints, the workflow was at times inefficient. For example, one dataset could have been used both for the EDA and the model, rather than separating the 2. The model was one of the biggest blockers in this project and I decided to leave the dataset for the EDA as a finished product before completing the model, as a plan B in case the model never worked. Finally the model did work but I lacked the time to integrate everything into one smooth process, hence the 2 step process. 

2. Some columns and features were explored during EDA but never used for visualisation; these are the division of mechanical and electric bikes, and the column showing if the day was on the eve of a weekend or bank holiday. The latter of these was still used to train the model, so it was used to a limited extent. With more time for cleaning my dataset I would probably have removed these, provided it didn't affect the model too much. The choice not to analyse the use of mechanical vs electric bikes is due to the comparatively low numbers of electric bikes, which in my opinion would not have been a big enough sample to draw conclusions from. 

3. Finding the right model was the biggest blocker, and took far longer than expected. I expected to use a Time Series model, meaning it would be based on the DateTime data (i.e. the exact timestamps). There are models such as Prophet and ARIMAX which can combine DateTime features and categorical features like the ones I ended up using. I tried all of these models and always ended up with a negative R2 score, meaning the model had less predictive accuracy than just taking the mean of all the values. So the model produced 2 setbacks; it took a lot of time away from other processes, and it was not as accurate as I would have hoped. 

4. The project is intended as a way of exploring how existing data can train predictive models to fill gaps in inconsistent or patchy historical data. In this case, using 5 months to predict 4 years' worth of data is unrealistic, as it would be too extreme a case. Obviously, the predictions I make based on the 5 months between August and December 2018 are probably no longer accurate after approximately a year, since factors like expansion or improvement of the service could lead to a global increase in use. In a more realistic scenario, we could use 2 years' worth of data to fill gaps of 3-4 months, for example.

### Next Steps ###



