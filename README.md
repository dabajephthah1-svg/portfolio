# portfolio

<a href="weather.html">Weather App</a>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Weather App</title>
    <link rel="stylesheet" href="exercise.css">
</head>
<body>

    <h1>Weather App</h1>

    <nav>
        <a href="index.html">Back to Portfolio</a>
    </nav>

    <p>Current weather data from a public API.</p>

    <!-- Buttons -->
    <button id="btnKuopio">Kuopio</button>
    <button id="btnHelsinki">Helsinki</button>

    <!-- Weather Display -->
    <div id="weatherBox">
        <p><strong>City:</strong> <span id="city">-</span></p>
        <p><strong>Temperature:</strong> <span id="temperature">-</span></p>
        <p><strong>Wind Speed:</strong> <span id="wind">-</span></p>
    </div>

    <!-- Debug Output -->
    <pre id="output"></pre>

    <script src="weather.js"></script>
</body>
</html>


JS
const cityText = document.getElementById("city")
const temperatureText = document.getElementById("temperature")
const windText = document.getElementById("wind")
const output = document.getElementById("output")

function log(message) {
    output.textContent += message + "\n"
}

function clearOutput() {
    output.textContent = ""
}

// Button events
document.getElementById("btnKuopio").onclick = function () {
    loadWeatherByCity("Kuopio", 62.8924, 27.6770)
}

document.getElementById("btnHelsinki").onclick = function () {
    loadWeatherByCity("Helsinki", 60.1699, 24.9384)
}

// Fetch weather data
async function loadWeatherByCity(cityName, latitude, longitude) {
    clearOutput()

    try {
        const url =
            "https://api.open-meteo.com/v1/forecast?latitude=" +
            latitude +
            "&longitude=" +
            longitude +
            "&current=temperature_2m,wind_speed_10m"

        const response = await fetch(url)

        if (!response.ok) {
            throw new Error("HTTP Error: " + response.status)
        }

        const data = await response.json()

        const temperature = data.current.temperature_2m
        const wind = data.current.wind_speed_10m

        // Update UI
        cityText.textContent = cityName
        temperatureText.textContent = temperature + " °C"
        windText.textContent = wind + " km/h"

        // Log output
        log("City: " + cityName)
        log("Temperature: " + temperature + " °C")
        log("Wind Speed: " + wind + " km/h")

    } catch (error) {
        log("Error: " + error.message)
    }
}

CSS
#weatherBox {
    margin-top: 20px;
    padding: 10px;
    border: 1px solid #ccc;
    width: 250px;
}

button {
    margin: 5px;
    padding: 10px;
    cursor: pointer;
}
