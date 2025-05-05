---
title: Mass–Luminosity Calculator
hide-nav: true
hide-toc: true
hide-footer: true
---

```{raw} html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Mass–Luminosity Calculator</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.7/dist/katex.min.css">
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background-color: #f4f4f4;
    }
    .calculator-container {
      width: 100%;
      max-width: 800px;
      margin: 0 auto;
      padding: 20px;
      background-color: white;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }
    .section {
      margin-bottom: 30px;
      padding: 20px;
      border: 1px solid #ccc;
      border-radius: 8px;
    }
    input {
      margin: 5px;
      padding: 5px;
      width: 120px;
    }
    button {
      padding: 8px 16px;
      margin-top: 10px;
      cursor: pointer;
    }
    .output {
      margin-top: 20px;
    }
  </style>
</head>
<body>

  <div class="calculator-container">
    <div class="section">
      <h2>Compute Luminosity from Mass</h2>
      <label>Mass (M):</label>
      <input type="number" id="massInput" step="any"><br>
      <label>Hydrogen Fraction (X):</label>
      <input type="number" id="hydrogenInput1" step="any"><br>
      <label>Metallicity (Z):</label>
      <input type="number" id="ZInput1" step="any"><br>
      <button id="computeLuminosityBtn">Compute Luminosity</button>
      <div class="output" id="luminosityResult"></div>
    </div>

    <div class="section">
      <h2>Compute Mass from Luminosity</h2>
      <label>Luminosity (L):</label>
      <input type="number" id="luminosityInput" step="any"><br>
      <label>Hydrogen Fraction (X):</label>
      <input type="number" id="hydrogenInput2" step="any"><br>
      <label>Metallicity (Z):</label>
      <input type="number" id="ZInput2" step="any"><br>
      <button id="computeMassBtn">Compute Mass</button>
      <div class="output" id="massResult"></div>
    </div>
  </div>

  <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.7/dist/katex.min.js"></script>
  <script>
    document.addEventListener('DOMContentLoaded', function () {
      document.getElementById('computeLuminosityBtn').addEventListener('click', getLuminosity);
      document.getElementById('computeMassBtn').addEventListener('click', getMass);
    });

    function renderLatex(targetId, content) {
      const el = document.getElementById(targetId);
      el.innerHTML = '';
      const lines = content.split('<br>');
      lines.forEach(line => {
        const span = document.createElement('div');
        katex.render(line, span, { throwOnError: false });
        el.appendChild(span);
      });
    }

    async function getLuminosity() {
      const m = parseFloat(document.getElementById('massInput').value);
      const x = parseFloat(document.getElementById('hydrogenInput1').value);
      const Z = parseFloat(document.getElementById('ZInput1').value);
      if (isNaN(m) || isNaN(x) || isNaN(Z)) return alert("Invalid input");

      try {
        const res = await fetch("https://nnv5wacde8.execute-api.eu-north-1.amazonaws.com/ML-calc", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ choice: "1", m, x, Z })
        });
        const data = await res.json();
        const latex = 
          "\\text{Minimum } \\log(L/L_\\odot):\\ " + data.L_min.toFixed(5) + "<br>" +
          "\\text{Maximum } \\log(L/L_\\odot):\\ " + data.L_max.toFixed(5) + "<br>" +
          "\\text{Pure He } \\log(L/L_\\odot):\\ " + data.Pure_He_Luminosity.toFixed(5);
        renderLatex("luminosityResult", latex);
      } catch (err) {
        alert("Failed: " + err);
      }
    }

    async function getMass() {
      const L = parseFloat(document.getElementById('luminosityInput').value);
      const x = parseFloat(document.getElementById('hydrogenInput2').value);
      const Z = parseFloat(document.getElementById('ZInput2').value);
      if (isNaN(L) || isNaN(x) || isNaN(Z)) return alert("Invalid input");

      try {
        const res = await fetch("https://nnv5wacde8.execute-api.eu-north-1.amazonaws.com/ML-calc", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ choice: "2", L, x, Z })
        });
        const data = await res.json();
        const latex = 
          "\\text{Minimum mass } (M/M_\\odot):\\ " + data.M_min + "<br>" +
          "\\text{Maximum mass } (M/M_\\odot):\\ " + data.M_max + "<br>" +
          "\\text{Pure He mass } (M/M_\\odot):\\ " + data.Pure_He_Mass;
        renderLatex("massResult", latex);
      } catch (err) {
        alert("Failed: " + err);
      }
    }
  </script>

</body>
</html>
