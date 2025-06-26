# Weather-forecasting-app
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Weather App</title>
  <style>
    * {
      box-sizing: border-box;
    }
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: linear-gradient(to right, #00c6ff, #0072ff);
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      color: #333;
    }
    .weather-container {
      background: #ffffff;
      padding: 3rem 2rem;
      border-radius: 2rem;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
      text-align: center;
      max-width: 500px;
      width: 100%;
      animation: fadeIn 1s ease-in-out;
    }
    h2 {
      margin-bottom: 1rem;
      color: #0072ff;
    }
    input[type="text"] {
      padding: 0.75rem;
      font-size: 1rem;
      border-radius: 1rem;
      border: 1px solid #ccc;
      width: 80%;
      margin-bottom: 1rem;
      outline: none;
      transition: 0.3s;
    }
    input[type="text"]:focus {
      border-color: #0072ff;
    }
    button {
      padding: 0.75rem 2rem;
      font-size: 1rem;
      margin-top: 0.5rem;
      border: none;
      background-color: #0072ff;
      color: white;
      border-radius: 1rem;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }
    button:hover {
      background-color: #005fcc;
    }
    #result {
      margin-top: 2rem;
      font-size: 1.1rem;
      line-height: 1.5;
      color: #444;
      animation: fadeInUp 0.6s ease-out;
    }
    .icon {
      margin-top: 1rem;
    }
    .forecast {
      margin-top: 2rem;
      background: #f4f4f4;
      padding: 1rem;
      border-radius: 1rem;
      animation: fadeInUp 0.8s ease-out;
    }
    @keyframes fadeIn {
      from { opacity: 0; transform: scale(0.95); }
      to { opacity: 1; transform: scale(1); }
    }
    @keyframes fadeInUp {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }
  </style>
</head>
<body>
  <div class="weather-container">
    <h2>🌦️ Weather Checker</h2>
    <input type="text" id="locationInput" placeholder="Enter location" />
    <br>
    <button onclick="getWeather()">Get Weather</button>
    <div id="result"></div>
    <div id="forecast"></div>
  </div>

  <script>
    function getWeather() {
      const location = document.getElementById('locationInput').value;
      const apiKey = '90a5782874a24ab9b3d144423252306';

      const currentUrl = `http://api.weatherapi.com/v1/current.json?key=${apiKey}&q=${location}&aqi=yes`;
      const forecastUrl = `http://api.weatherapi.com/v1/forecast.json?key=${apiKey}&q=${location}&days=3&aqi=no&alerts=no`;

      // Fetch current weather
      fetch(currentUrl)
        .then(response => {
          if (!response.ok) throw new Error('Location not found');
          return response.json();
        })
        .then(data => {
          const tempC = data.current.temp_c;
          const condition = data.current.condition.text;
          const iconUrl = data.current.condition.icon;
          document.getElementById('result').innerHTML = `
            <strong>📍 Location:</strong> ${data.location.name}, ${data.location.country}<br>
            <strong>🌡️ Temperature:</strong> ${tempC}°C<br>
            <strong>🌤️ Condition:</strong> ${condition}<br>
            <div class="icon"><img src="https:${iconUrl}" alt="weather icon"></div>
          `;
        })
        .catch(error => {
          document.getElementById('result').innerText = error.message;
        });

      // Fetch 3-day forecast
      fetch(forecastUrl)
        .then(response => response.json())
        .then(data => {
          const forecastDays = data.forecast.forecastday;
          let forecastHTML = '<h3>🔮 3-Day Forecast</h3>';
          forecastDays.forEach(day => {
            forecastHTML += `
              <div>
                <strong>${day.date}</strong><br>
                🌤️ ${day.day.condition.text}<br>
                🌡️ Max: ${day.day.maxtemp_c}°C, Min: ${day.day.mintemp_c}°C<br>
                <img src="https:${day.day.condition.icon}" alt="icon">
              </div>
              <hr>
            `;
          });
          document.getElementById('forecast').innerHTML = `<div class="forecast">${forecastHTML}</div>`;
        });
    }
  </script>
</body>
</html>
