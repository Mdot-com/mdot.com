---
layout: default
title: Mass-Luminosity Calculator
---

<style>
  body {
    padding: 20px;
    text-align: center;
  }

  h1, h2, p, label {
    margin-bottom: 15px;
  }

  #luminosity-form {
    margin-bottom: 20px;
    display: inline-block;
    text-align: left;
  }

  input, button {
    margin-top: 5px;
    width: 200px; /* Set a fixed width for the input boxes */
    padding: 5px;
    text-align: center;
  }

  #luminosity-output {
    padding: 20px;
    border: 1px solid #ccc;
    margin-top: 20px;
    background-color: #f9f9f9;
    width: 300px;
    margin-left: auto;
    margin-right: auto;
  }
</style>


Welcome to the mass-luminosity (ML) calculator page. Here we provide a web interface version for calculating minimum, maximum and pure-He mass-luminosity fit relations based on the structure models in Sabhahit et al. (2025b). The novelty of this work 



<form id="luminosity-form">
    <label for="m">Mass (M):</label>
    <input type="number" id="m" name="m" step="any" required>
    <br><br>

    <label for="x">Hydrogen Mass Fraction (X):</label>
    <input type="number" id="x" name="x" step="any" required>
    <br><br>

    <label for="z">Metallicity (Z):</label>
    <input type="number" id="z" name="z" step="any" required>
    <br><br>

    <button type="button" id="calculate-luminosity">Calculate Luminosity</button>
</form>

### Results:
<div id="luminosity-output">
    <p>Results will appear here.</p>
</div>

<script>
    document.getElementById('calculate-luminosity').addEventListener('click', function() {
        const m = parseFloat(document.getElementById('m').value);
        const x = parseFloat(document.getElementById('x').value);
        const z = parseFloat(document.getElementById('z').value);

        if (!m || !x || !z) {
            alert('Please enter Mass (M), Hydrogen Mass Fraction (X), and Metallicity (Z).');
            return;
        }

        const data = {
            "choice": "1",
            "Z": z,
            "m": m,
            "x": x
        };

        fetch('https://nnv5wacde8.execute-api.eu-north-1.amazonaws.com/ML-calc', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(data)
        })
        .then(response => response.json())
        .then(data => {
            const output = document.getElementById('luminosity-output');
            if (data.Pure_He_Luminosity) {
                output.innerHTML = `
                    <p><strong>L_min:</strong> ${data.L_min}</p>
                    <p><strong>L_max:</strong> ${data.L_max}</p>
                    <p><strong>Pure_He_Luminosity:</strong> ${data.Pure_He_Luminosity}</p>
                `;
            } else {
                output.innerHTML = '<p style="color: red;">Error: Missing results</p>';
            }
        })
        .catch(error => {
            document.getElementById('luminosity-output').innerHTML = '<p style="color: red;">Error: ' + error.message + '</p>';
        });
    });
</script>
