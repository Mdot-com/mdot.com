---
layout: default
title: Minimum, maximum and pure-He Mass-Luminosity Calculator
---

<style>
  body {
    padding: 20px;
  }

  h1, h2, p, label {
    margin-bottom: 15px;
  }

  #luminosity-form {
    margin-bottom: 20px;
  }

  input, button {
    margin-top: 5px;
  }

  #luminosity-output {
    padding: 20px;
    border: 1px solid #ccc;
    margin-top: 20px;
    background-color: #f9f9f9;
  }
</style>

# Calculate Min, Max, and Pure He Luminosity for Given Mass (M), Hydrogen Mass Fraction (X), and Metal Mass Fraction (Z)


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
