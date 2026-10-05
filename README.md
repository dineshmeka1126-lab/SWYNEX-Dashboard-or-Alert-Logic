# SWYNEX-Dashboard-or-Alert-Logic

## Task 3 – Dashboard or Alert Logic

This project is a simple IoT dashboard created for SWYNEX Task 3. It displays simulated temperature data using a web-based dashboard and chart.

### Alert Rule

- Temperature > 30°C → ALERT
- Temperature ≤ 30°C → NORMAL

### Technologies Used

- HTML
- CSS
- JavaScript
- Simulated IoT Data

### Features

- Temperature monitoring
- Temperature trend chart
- Automatic alert detection
- Simple and responsive dashboard
- Simulated IoT sensor data

### How to Run

1. Download or clone this repository.
2. Open `index.html`.
3. The dashboard will open in a web browser.
4. Check the temperature readings and alert status.

### Project Objective

The objective is to demonstrate how IoT sensor data can be visualized and monitored using a simple dashboard with threshold-based alert logic.<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>SWYNEX IoT Dashboard</title>

    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f4f6f8;
            margin: 0;
            padding: 20px;
        }

        .container {
            max-width: 900px;
            margin: auto;
        }

        h1 {
            text-align: center;
            color: #222;
        }

        .dashboard {
            display: flex;
            gap: 20px;
            justify-content: center;
            flex-wrap: wrap;
            margin: 25px 0;
        }

        .card {
            background: white;
            padding: 25px;
            width: 220px;
            text-align: center;
            border-radius: 12px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
        }

        .value {
            font-size: 30px;
            font-weight: bold;
            margin-top: 10px;
        }

        .normal {
            color: green;
        }

        .alert {
            color: red;
        }

        .chart-box {
            background: white;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
        }

        canvas {
            width: 100%;
            max-width: 800px;
        }

        .message {
            margin-top: 20px;
            padding: 15px;
            text-align: center;
            border-radius: 8px;
            font-weight: bold;
        }
    </style>
</head>

<body>

<div class="container">

    <h1>SWYNEX IoT Dashboard</h1>

    <div class="dashboard">

        <div class="card">
            <div>Latest Temperature</div>
            <div class="value" id="temperature">-- °C</div>
        </div>

        <div class="card">
            <div>Alert Threshold</div>
            <div class="value">30 °C</div>
        </div>

        <div class="card">
            <div>System Status</div>
            <div class="value" id="status">--</div>
        </div>

    </div>

    <div class="chart-box">

        <h2>Temperature Trend</h2>

        <canvas id="temperatureChart"
                width="800"
                height="350">
        </canvas>

    </div>

    <div class="message" id="message"></div>

</div>

<script>

    // Simulated IoT temperature data
    const temperatureData = [
        27,
        28,
        29,
        31,
        30,
        28,
        32,
        29
    ];

    // Alert threshold
    const threshold = 30;

    // Get latest temperature
    const latestTemperature =
        temperatureData[temperatureData.length - 1];

    // Display temperature
    document.getElementById("temperature")
        .textContent = latestTemperature + " °C";


    // Alert logic
    const status = document.getElementById("status");
    const message = document.getElementById("message");

    if (latestTemperature > threshold) {

        status.textContent = "ALERT";
        status.className = "value alert";

        message.textContent =
            "ALERT: Temperature is above 30°C.";

        message.style.background = "#ffd6d6";
        message.style.color = "red";

    } else {

        status.textContent = "NORMAL";
        status.className = "value normal";

        message.textContent =
            "NORMAL: Temperature is within the safe limit.";

        message.style.background = "#d8f5dc";
        message.style.color = "green";
    }


    // Draw chart
    const canvas =
        document.getElementById("temperatureChart");

    const ctx = canvas.getContext("2d");

    const width = canvas.width;
    const height = canvas.height;

    const padding = 50;

    const minTemp = 20;
    const maxTemp = 35;


    // Draw horizontal grid lines
    ctx.strokeStyle = "#dddddd";
    ctx.lineWidth = 1;

    for (let temp = 20; temp <= 35; temp += 5) {

        const y =
            height -
            padding -
            ((temp - minTemp) /
            (maxTemp - minTemp)) *
            (height - 2 * padding);

        ctx.beginPath();
        ctx.moveTo(padding, y);
        ctx.lineTo(width - padding, y);
        ctx.stroke();

        ctx.fillStyle = "#555";
        ctx.font = "12px Arial";

        ctx.fillText(temp + "°C", 10, y + 4);
    }


    // Draw alert threshold line
    const thresholdY =
        height -
        padding -
        ((threshold - minTemp) /
        (maxTemp - minTemp)) *
        (height - 2 * padding);

    ctx.strokeStyle = "red";
    ctx.setLineDash([8, 5]);

    ctx.beginPath();
    ctx.moveTo(padding, thresholdY);
    ctx.lineTo(width - padding, thresholdY);
    ctx.stroke();

    ctx.setLineDash([]);


    // Draw temperature line
    ctx.strokeStyle = "blue";
    ctx.lineWidth = 3;

    ctx.beginPath();

    temperatureData.forEach((temp, index) => {

        const x =
            padding +
            index *
            ((width - 2 * padding) /
            (temperatureData.length - 1));

        const y =
            height -
            padding -
            ((temp - minTemp) /
            (maxTemp - minTemp)) *
            (height - 2 * padding);

        if (index === 0) {
            ctx.moveTo(x, y);
        } else {
            ctx.lineTo(x, y);
        }
    });

    ctx.stroke();


    // Draw data points
    temperatureData.forEach((temp, index) => {

        const x =
            padding +
            index *
            ((width - 2 * padding) /
            (temperatureData.length - 1));

        const y =
            height -
            padding -
            ((temp - minTemp) /
            (maxTemp - minTemp)) *
            (height - 2 * padding);

        ctx.fillStyle =
            temp > threshold ? "red" : "blue";

        ctx.beginPath();

        ctx.arc(x, y, 5, 0, Math.PI * 2);

        ctx.fill();
    });

</script>

</body>
</html># SWYNEX-Dashboard-or-Alert-Logic
This project is a simple IoT dashboard created for SWYNEX Task 3. It visualizes simulated temperature data and displays a temperature trend chart. An alert is triggered when the temperature exceeds **30°C**. The project uses **HTML, CSS, and JavaScript** to demonstrate IoT data visualization and threshold-based alert logic.
