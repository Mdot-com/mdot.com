---
layout: default
title: Mass–Luminosity Calculator
---

# Mass–Luminosity Calculator

Welcome to the **ML Calculator**! This tool allows you to input values for **Mass (M)**, **Luminosity (L)**, **Helium abundance (X)**, and **Metallicity (Z)**, and it will calculate the corresponding **Luminosity (L)** or **Mass (M)** based on the chosen option.

## Inputs

Please select the appropriate option below and provide the necessary values:

### Choice 1: Luminosity Calculation (L)
- **Mass (M)**: The mass of the star.
- **Helium abundance (X)**: The helium abundance in the star.
- **Metallicity (Z)**: The metallicity in the star.

### Choice 2: Mass Calculation (M)
- **Luminosity (L)**: The luminosity of the star.
- **Helium abundance (X)**: The helium abundance in the star.
- **Metallicity (Z)**: The metallicity in the star.

## Form for Calculation

<form id="calculator-form">
    <label for="choice">Choice:</label>
    <select id="choice" name="choice">
        <option value="1">Luminosity Calculation (L)</option>
        <option value="2">Mass Calculation (M)</option>
    </select>
    <br><br>

    <label for="m">Mass (M):</label>
    <input type="number" id="m" name="m" step="any">
    <br><br>

    <label for="l">Luminosity (L):</label>
    <input type="number" id="l" name="l" step="any">
    <br><br>

    <label for="x">Helium abundance (X):</label>
    <input type="number" id="x" name="x" step="any">
    <br><br>

    <label for="z">Metallicity (Z):</label>
    <input type="number" id="z" name="z" step="any">
    <br><br>

    <button type="button" onclick="calculate()">Calculate</button>
</form>

## Results

<div id="output">
    <p>Results will appear here.</p>
</div>

<script>
    function calculate() {
        const choice = document.getElementById('choice').value;
        const m = parseFloat(document.getElementById('m').value);
        const l = parseFloat(document.getElementById('l').value);
        const x = parseFloat(document.getElementById('x').value);
        const z = parseFloat(document.getElementById('z').value);

        let data = {};

        if (choice === '1') {
            if (!m || !x || !z) {
                alert('Please enter Mass (M), Helium abundance (X), and Metallicity (Z) for Luminosity calculation.');
                return;
            }
            data = {
                "choice": "1",
                "Z": z,
                "m": m,
                "x": x
            };
        } else if (choice === '2') {
            if (!l || !x || !z) {
                alert('Please enter Luminosity (L), Helium abundance (X), and Metallicity (Z) for Mass calculation.');
                return;
            }
            data = {
                "choice": "2",
                "Z": z,
                "L": l,
                "x": x
            };
        }

        fetch('https://nnv5wacde8.execute-api.eu-north-1.amazonaws.com/ML-calc', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(data)
        })
        .then(response => response.json())
        .then(data => {
            let output = document.getElementById('output');
            if (choice === '1') {
                output.innerHTML = `
                    <p><strong>L_min:</strong> ${data.L_min}</p>
                    <p><strong>L_max:</strong> ${data.L_max}</p>
                    <p><strong>Pure_He_Luminosity:</strong> ${data.Pure_He_Luminosity}</p>
                `;
            } else if (choice === '2') {
                output.innerHTML = `
                    <p><strong>M_min:</strong> ${data.M_min}</p>
                    <p><strong>M_max:</strong> ${data.M_max}</p>
                    <p><strong>Pure_He_Mass:</strong> ${data.Pure_He_Mass}</p>
                `;
            }
        })
        .catch(error => {
            document.getElementById('output').innerHTML = '<p style="color: red;">Error: ' + error.message + '</p>';
        });
    }
</script>

