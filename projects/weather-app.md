# Week 7 Build a Minimal APP connecting backend and frontend  Project

### Frameworks

- Backend: ExpressJS
- Frontend: React (we suggest that you use create-react-app)

### Libraries/Tools

- NodeJS 
- Express
- Postman
- Create-React-APP
- Fetch

### Primary Goals

This assignment will check for proficiency in: NodeJS, ExpressJS, and how connect your backend with the frontend

(We will cover Database, API testing, and CRUD in future assessments.)

### Overview

In this project, you will build a minimal Weather App connecting your backend and your frontend. 

Win #1 
The weather app should have all the basic functions including: city name, current weather icon, temperature, humidity, wind speed, etc. 

Win #2
It should display the icon images for sunny/rainy/cloudy/snowy weather conditions. 

Win #3
It should have a responsive design.

### Context

- After learning Node, Express, and API concepts (Fetch),  you will use them to connect with a external API in the Backend. - We recommend that you use OpenWeather API ﻿https://openweathermap.org/api


  - Register for a student open weather map API account. Put Techtonica under institution and if they ask for documentation use your acceptance email. 
 
 <img width="856" alt="Open Weather Map Student" src="https://user-images.githubusercontent.com/102179075/221019420-14df523d-a7ed-4e6a-bdc6-e048bd7bb3fe.png">
  
  - You'll put N/A for student ID and a year from now for expiration is fine. 
  - A prompt should let you know that an email will come and the student registration should be applied in a few hours.
  - Please note that for this project we using the APi 2.5 that allows you to fetch current weather data using the name of the city. If you want to read the docummentation of the API please follow [this link](https://openweathermap.org/current#name)
  
  <img width="470" alt="After submit Open Weather Map Student" src="https://user-images.githubusercontent.com/102179075/221019725-346c5d71-c1d4-43fc-b600-1b1fdf3cc46d.png">
  

- You will use React to show the results of the API fetch to your frontend 
# Project Instructions

## Part 0 - Starter Code
### For the Backend
- [ ] Create your main directory for this project. Initialize this project folder for git, and get a new remote GitHub repo ready so you can save and push as you make progress on your project. You will be submitting your GitHub repo link once you finish.
- [ ] Inside the main folder of your project create a server folder 
- [ ] Inside the server folder setup a basic express server

### For the Frontend
- [ ] Inside your client folder initialize a create-react-app <client> in React with the command `npx create-react-app client` 
  
## Part 0.1 - Working with existing code
- [ ] Here is an app that is fully running useing hardcoded data. Start by cloneing this and get it running on your computer [Cristina's template for the Weather Project](https://github.com/Yosolita1978/HardCodeDataWeatherApi)
  
## Part 1 - Connecting the API in the backend
- [ ] Change the Express connection from using a hard coded data file, to connecting to the live API
- [ ] Inside the server folder in your server.js file do a fetch request to the Weather API
![Code Example](https://raw.githubusercontent.com/Yosolita1978/screenshoots/836e1da625022b836f2aef42b3cace63563782a7/Week7/Screen%20Shot%202022-09-05%20at%206.15.14%20PM.png)

## Part 2 - Connecting the API in the frontend
- [ ] Inside the client folder in your script.js file you must add fetch request to bring the data from the backend
- [ ] Render the weather infomation inside a component
- [ ] Choose at least 3 more pieces of information from the HTTP response to your React front end

## Guide code
You can see a guide code from Cristina working with hardcode data [here](https://github.com/Yosolita1978/HardCodeDataWeatherApi)
You can see a guide code from Cristina working with real data with a API_KEY[here](https://github.com/Yosolita1978/RealDataWeatherAPI)
