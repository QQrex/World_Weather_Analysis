# World_Weather_Analysis

## Purpose

PlanMyTrip app would like us to refactor their code to add weather description to their weather data and add it to their layered Google map pop-up markers. Also, they would like us to add a travel itinerary for their beta testers using Google Maps.

## Project Overview

- Retrieve Weather Data from random coordinates.

- Create a Customer Travel Destination Map with desription added to pop-up markers.

- Create a Travel Itineary Map using Google maps.

## Analysis

**[Data Request]**

To begin our analysis we would first need to generate some random coordinates to map. We will be using np.random to create random lats and lngs.

![DepGenCoord](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/dep%2C%20gen%20random%20lat.PNG)
> Cell 1 - Import dependencies.
> 
> Cell 2 - Generate random lats/lngs and then using zip() to create an iterator of tuples

Next we will create set our list of the lats/lngs tuples as coordinates. Then take the coordinates and find the nearest cities to them.

![CoordsCities](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/create%20coords%20and%20find%20cities.PNG)
>Cell 1 - Create list of of coordinates from our tuples of lats/lngs.
>
>Cell 2 Line 1 - Create empty list of cities.
>
>Cell 2 Line 6 to 12 - Loop through coordinates to find cities using citipy.nearest_city function and then appending to our empty citie list if not already in list.
>Cell 2 Line 16 - Count how many cities in our cities list.
>

We will be using openweathermap.org API to get our weather data. Since we have 759 cities in our list, it is a good idea to do a test run of just one city to ensure we are able to request data from the API and store the relevant data in variables.

![TestCall](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/Test%20call%20and%20parse.PNG)
>Cell 1 - Create url variable with API website + API key.
>
>Cell 2 - Test to see if we are able to request data in json format using the city of Boston as an example.
>
>Cell 3 - Check to see if we are able to extract json data into variables and print example.

Now that we checked our code is able to request data, we need to create an empty list of city data and create a record/set counter starting at 1.

![datalistCounters](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/create%20list%20and%20counters.PNG)

We need to create a record and set counter because the API we are requesting our data from has a 60 second counter that will timeout our request. This is done to prevent endless request of data from the API service so in order to bypass the timeout, we are creating a counter that will put our request to sleep for 60 secs after it has requested data for 50 cities (1 set of cities). Also, we don't want our request to stop prematurely due to a city in our list to not have weather data in the API. We can avoid this by using try and except statement in our script to skip cities with no data. Finally, we will also add print statement along the way as a visual for us to know the script is working.

![loopURL](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/loop%20to%20loop%20through%20url.PNG)
>Line 1 - Loop through both index and cities in our cities list with enumerate().
>
>Line 6 to 9 - If statment for if the index divide by 50 remainder is equal to 0 and if index is greater than 50 is true, our set counter will increase by 1 and our counter will reset to 1. Also, our script will sleep for 60 seconds.
>
>line 12 - Request data from our city and also using .replace() in our city to remove spaces and concatenating our city name.
>
>Line 14 - Print statement to know our script is requesting data.
>
>Line 15 - Increase record counter by 1
>
>Line 22 to 53 - Try and except statment to request data in json format and then extracting relevant data and appending it to our city_data list.
>
>Line 56 to 58 - Print statment to show our script has finish.

Now we count our city_data list to see how cities we have in our list and convert our list into a DataFrame. We will also like to rearrange our columns and check for NaN dtat with isnull(). Finally we will save our output date in CSV format so if we want to use this dataset again, we wouldn't have to repeat the API request.

![outputdata](https://github.com/QQrex/World_Weather_Analysis/blob/main/Resources/create%20output%20data%20df%20and%20save.PNG)

**[Travel Destination Map]**

