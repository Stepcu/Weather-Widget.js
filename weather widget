// Import necessary React hooks and the Axios library
import React, { useState, useEffect } from "react";
import axios from "axios";

// Icons for different weather conditions during the day and night
const WeatherIcons = {
  day: {
    Clear: "/weather_icons/sun.gif",
    Clouds: "/weather_icons/clouds.gif",
    Rain: "/weather_icons/rain.gif",
    Drizzle: "/weather_icons/drizzle.gif",
    Thunderstorm: "/weather_icons/storm.gif",
    Snow: "/weather_icons/snow.gif",
    Mist: "/weather_icons/mist.gif",
  },
  night: {
    Clear: "/weather_icons/moon.gif",
    Clouds: "/weather_icons/clouds-night.gif",
    Rain: "/weather_icons/rain.gif",
    Drizzle: "/weather_icons/drizzle-night.gif",
    Thunderstorm: "/weather_icons/storm.gif",
    Snow: "/weather_icons/snow.gif",
    Mist: "/weather_icons/mist.gif",
  },
};

function WeatherWidget() {
  const [weather, setWeather] = useState(null); // Stores weather data
  const [isNight, setIsNight] = useState(false); // Determines if it's night
  const API_KEY = ""; // OpenWeather API key

  // Get user's location and fetch weather data
  useEffect(() => {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        (position) => {
          const { latitude, longitude } = position.coords;
          fetchWeather(latitude, longitude); // Call function to fetch weather data
        },
        (error) => {
          console.error("Error getting location:", error);
        }
      );
    } else {
      console.error("Geolocation is not supported by this browser.");
    }
  }, []);

  // Fetch weather data based on user coordinates
  const fetchWeather = (lat, lon) => {
    axios
      .get(
        `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${API_KEY}&units=metric&lang=es`
      )
      .then((response) => {
        console.log("API Data:", response.data);
        setWeather(response.data);

        // Determine if it's currently night
        const currentTime = new Date().getTime() / 1000; // Current time in seconds
        const { sunrise, sunset } = response.data.sys; // Sunrise and sunset times
        setIsNight(currentTime < sunrise || currentTime > sunset); // True if before sunrise or after sunset
      })
      .catch((error) => console.error("Error fetching weather data:", error));
  };

  return (
    <div style={{ fontFamily: "Roboto, sans-serif" }}>
      {weather ? (
        <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", gap: "15px" }}>
          <div>
            {/* Display weather icon based on the condition */}
            {WeatherIcons[isNight ? "night" : "day"][weather.weather[0].main] ? (
              <img
                src={WeatherIcons[isNight ? "night" : "day"][weather.weather[0].main]}
                alt={weather.weather[0].main}
                style={{ width: "30px", height: "30px" }}
              />
            ) : (
              <p>❓</p> // Default icon if condition is unknown
            )}
          </div>
          <p>{weather.name}</p> {/* Display city name */}
          <p>{Math.round(weather.main.temp)}°C</p> {/* Display temperature */}
        </div>
      ) : (
        <p>Fetching location and weather...</p> // Loading state
      )}
    </div>
  );
}

export default WeatherWidget;
