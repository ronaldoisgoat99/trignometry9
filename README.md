<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Unit Circle Calculator</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #eef2f7;
      text-align: center;
      padding: 20px;
    }
    h1 {
      color: #2c3e50;
      margin-bottom: 10px;
    }
    input, select, button {
      padding: 10px;
      margin: 10px;
      font-size: 16px;
    }
    canvas {
      border: 1px solid #ccc;
      background: #fff;
      margin-top: 20px;
    }
    #results {
      margin-top: 20px;
      font-size: 18px;
    }
    footer {
      margin-top: 50px;
      font-size: 14px;
      color: #555;
    }
  </style>
</head>
<body>
  <h1>🧮 Interactive Unit Circle Calculator</h1>

  <label for="angle">Angle:</label>
  <input type="number" id="angle" value="45" />
  <select id="unit">
    <option value="degrees">Degrees</option>
    <option value="radians">Radians</option>
  </select>

  <br>

  <label for="slider">🎚️ Angle Slider:</label><br>
  <input type="range" id="slider" min="0" max="360" value="45" oninput="syncSlider(this.value)" style="width: 300px;">

  <br>
  <button onclick="calculate()">Calculate</button>

  <div id="results"></div>

  <canvas id="unitCircle" width="300" height="300"></canvas>

  <footer>
    <hr>
    Made by Sarthak · Hosted on GitHub
  </footer>

  <script>
    const canvas = document.getElementById('unitCircle');
    const ctx = canvas.getContext('2d');
    const radius = 120;
    const centerX = canvas.width / 2;
    const centerY = canvas.height / 2;

    function toRadians(angle) {
      return angle * (Math.PI / 180);
    }

    function drawCircle(angleDeg) {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      // Draw circle
      ctx.beginPath();
      ctx.arc(centerX, centerY, radius, 0, 2 * Math.PI);
      ctx.strokeStyle = "#2c3e50";
      ctx.lineWidth = 2;
      ctx.stroke();

      // Axes
      ctx.beginPath();
      ctx.moveTo(centerX - radius, centerY);
      ctx.lineTo(centerX + radius, centerY);
      ctx.moveTo(centerX, centerY - radius);
      ctx.lineTo(centerX, centerY + radius);
      ctx.strokeStyle = "#aaa";
      ctx.stroke();

      // Line for angle
      const angleRad = toRadians(angleDeg);
      const x = centerX + radius * Math.cos(angleRad);
      const y = centerY - radius * Math.sin(angleRad); // canvas y is inverted

      ctx.beginPath();
      ctx.moveTo(centerX, centerY);
      ctx.lineTo(x, y);
      ctx.strokeStyle = "red";
      ctx.lineWidth = 2;
      ctx.stroke();

      // Dot at (x, y)
      ctx.beginPath();
      ctx.arc(x, y, 4, 0, 2 * Math.PI);
      ctx.fillStyle = "red";
      ctx.fill();
    }

    function syncSlider(value) {
      document.getElementById("angle").value = value;
      calculate();
    }

    function calculate() {
      let angleInput = parseFloat(document.getElementById("angle").value);
      const unit = document.getElementById("unit").value;

      if (isNaN(angleInput)) {
        document.getElementById("results").innerHTML = "Please enter a valid angle.";
        return;
      }

      if (unit === "radians") {
        angleInput = angleInput * (180 / Math.PI); // convert to degrees for drawing
      }

      const radians = unit === "degrees" ? toRadians(angleInput) : angleInput;

      const sin = Math.sin(radians).toFixed(4);
      const cos = Math.cos(radians).toFixed(4);
      const tan = Math.tan(radians);
      let tanDisplay = Math.abs(Math.cos(radians)) < 1e-10 ? "Undefined" : tan.toFixed(4);

      document.getElementById("results").innerHTML = `
        <strong>Results:</strong><br>
        sin(θ) = ${sin}<br>
        cos(θ) = ${cos}<br>
        tan(θ) = ${tanDisplay}
      `;

      drawCircle(angleInput);
    }

    // Initial render
    window.onload = () => {
      calculate();
    };
  </script>
</body>
</html>
