# World_Weather_Analysis

## Purpose

PlanMyTrip app would like us to refactor their code to add weather description to their weather data and add it to their layered Google map pop-up markers. Also, they would like us to add a travel itinerary for their beta testers using Google Maps.

## Project Overview

- Retrieve Weather Data from random coordinates.

- Create a Customer Travel Destination Map with description added to pop-up markers.

- Create a Travel Itinerary Map and mark layer map using Google maps.

## Analysis

**[Data Request]**

To begin our analysis, we would first need to generate some random coordinates to map. We will be using np.random to create random lats and lngs.

![DepGenCoord](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/dep%2C%20gen%20random%20lat.PNG)
> Cell 1 - Import dependencies.
> 
> Cell 2 - Generate random lats/lngs and then using zip() to create an iterator of tuples

Next, we will create set our list of the lats/lngs tuples as coordinates. Then take the coordinates and find the nearest cities to them.

![CoordsCities](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/create%20coords%20and%20find%20cities.PNG)
>Cell 1 - Create list of coordinates from our tuples of lats/lngs.
>
>Cell 2, line 1 - Create empty list of cities.
>
>Cell 2, line 6 to 12 - Loop through coordinates to find cities using citipy.nearest_city function and then appending to our empty cities list if not already in list.
>
>Cell 2, line 16 - Count how many cities in our cities list.
>

We will be using openweathermap.org API to get our weather data. Since we have 759 cities in our list, it is a good idea to do a test run of just one city to ensure we are able to request data from the API and store the relevant data in variables.

![TestCall](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/Test%20call%20and%20parse.PNG)
>Cell 1 - Create url variable with API website + API key.
>
>Cell 2 - Test to see if we can request data in json format using the city of Boston as an example.
>
>Cell 3 - Check to see if we can extract json data into variables and print example.

Now that we checked our code can request data, we need to create an empty list of city data and create a record/set counter starting at 1.

![datalistCounters](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/create%20list%20and%20counters.PNG)

We need to create a record and set counter because the API we are requesting our data from has a 60 second counter that will timeout our request. This is done to prevent endless request of data from the API service so in order to bypass the timeout, we are creating a counter that will put our request to sleep for 60 secs after it has requested data for 50 cities (1 set of cities). Also, we don't want our request to stop prematurely due to a city in our list to not have weather data in the API. We can avoid this by using try and except statement in our script to skip cities with no data. Finally, we will also add print statement along the way as a visual for us to know the script is working.

![loopURL](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/loop%20to%20loop%20through%20url.PNG)
>Line 1 - Loop through both index and cities in our cities list with enumerate().
>
>Line 6 to 9 - If statement for if the index divide by 50 remainder is equal to 0 and if index is greater than 50 is true, our set counter will increase by 1 and our counter will reset to 1. Also, our script will sleep for 60 seconds.
>
>line 12 - Request data from our city and also using .replace() in our city to remove spaces and concatenating our city name.
>
>Line 14 - Print statement to know our script is requesting data.
>
>Line 15 - Increase record counter by 1
>
>Line 22 to 53 - Try and except statement to request data in json format and then extracting relevant data and appending it to our city_data list.
>
>Line 56 to 58 - Print statment to show our script has finish.

Now we count our city_data list to see how cities we have in our list and convert our list into a DataFrame. We will also like to rearrange our columns and check for NaN values with isnull(). Finally we will save our output date in CSV format so if we want to use this dataset again, we wouldn't have to repeat the API request.

![outputdata](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/create%20output%20data%20df%20and%20save.PNG)

**[Travel Destination Map]**

Our next step will be to generate a travel destination map using Google maps with pop-up markers with nearest hotel/location/descriptions. We start our code in a new file so we will have to load our dependencies again and create a path to open our stored weather data from our openweathermap.org API request.

![dep,loadcity](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/dep%2C%20load%20city%20data.PNG)

Next, we would like to create an input for user to select preferred temperature range of a city they would like to travel to and create a new DataFrame of possible cities from the input.

![preftemp](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/pref%20temp.PNG)

With the preferred temp DataFrame, we would like to check to make sure none of the values are blank. Our dataset seems to have a Country that is blank so we will use .dropna() to remove the entire row. Next, we will use .copy() to copy our Dataframe to a new hotel_df DataFrame and also add a column to put the hotel names.

![dropnahotelname](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/dropna%2C%20hotel%20name.PNG)

Now that our hotel_df is set up, we need to make a data request to Google API to find closest hotels to our city coordinates. Since our Lats and Lngs are no longer in a list of tuples and we have multiple parameters for our request we will use paramas in our requests.get() request.

![findhotel](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/find%20hotel%20name.PNG)
>Line 2 to 6 - Set parameters for params.
>
>Line 9 - Loop to interate through index/row of hotel_df.
>
>Line 11 to 14 - Set lat and lng from hotel_df and adding to params.
>
>Line 17 - URL for Google API.
>
>Line 20 - Request data with params in json format.
>
>Line 23 to 26 - Using try and accept, add hotel name if hotel found.

Next, we should check our DataFrame to see if there are any hotels with NaN values and then remove the entire row if there are. We will also save our hotel_df as a csv file in case we need to go revisit this data and not have to do another API request.

![isnullsave](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/check%20na%20and%20save%20hotel%20output.PNG)

Finally, we need to we need to set up our info box template with the variables hotel name, city, country and current weather as a description for our Google map layer. We will generate a google map with a heat layer as well for all the max temperature for each city.

![infoboxGmap](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/add%20info%20box.PNG)
>Cell 1, line 2 to 9 - Info box template.
>
>Cell 1, line 12 - Loop to add values to info boxes for each row in our hotel_df.
>
>Cell 1, line 15 - Location variable as our lats and lngs from our hotel_df.
>
>Cell 2, line 2 - Max_temp variable as our max temps from our hotel_df.
>
>Cell 2, line 4 to 8 - Creating our googlemap with heat and marker layers.

**[Travel Itineary Map]**

The final two items are to create is a travel itinerary map and a marker layered map of 3 travel locations. We will once again start in a new file so we will import our dependencies again and load our WeatherPy_vacation.csv.

![depvacationdata](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/dep%2C%20load%20vacation%20data.PNG)

Next, we want to recreate our Travel Destination map and pick 3 locations we would like to travel to and back.

![refactor](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/deli%202%20refactor.PNG)
>Reuse our code from our previous file.

Once we decide the locations we would like to travel to, we need to pull the information from our DataFrame and add it to waypoint variables. We also need to take the lat and lng of each city and create a tuple pair as coordinates. Finally, we will create our travel itinerary map of our 3 destinations.

![travel](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/travel%20cities%20AU.PNG)
>Cell 1 - Using .loc to pull information from our vacation_df of our travel locations.
>
>Cell 2 - Using to_numpy() to create a lat and lng pair as a tuple.
>
>Cell 3 - Create a Google map figure of our travel destinations.

To create the our travel marker layered map, we need to concatenate our travel locations into a DataFrame and reuse our code from when we created a layered map again.

![traveldes](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/gmap%20of%20travel%20des.PNG)
>Cell 1 - Concatenate our waypoints into one DataFrame.
>
>Cell 2 and 3 - Reused code to create mark layered map.

## Results

![vacationmap](https://github.com/QQrex/World_Weather_Analysis/blob/main/Vacation_Search/WeatherPy_vacation_map.png)
>WeatherPy vacation map.

![travelmap](https://github.com/QQrex/World_Weather_Analysis/blob/main/Vacation_Itinerary/WeatherPy_travel_map.png)
>WeatherPy travel itineary map.

![travelmapmarker](https://github.com/QQrex/World_Weather_Analysis/blob/main/Vacation_Itinerary/WeatherPy_travel_map_markers.png)
>WeatherPy travel map with markers.

## Summary

In summary, the script we wrote will allow PlanMyTrip app customers to search possible travel locations on a map, depending on temperature preference, with description markers. From the possible travel locations, the customers can pick 3 locations to travel to and back as well.
