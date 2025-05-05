---
layout: default
title: Mass–Luminosity Calculator
---

# Mass–Luminosity Calculator

Welcome to the **ML Calculator**! You can calculate either the **Luminosity (L)** from the star's **Mass (M)** or the **Masses (M)** from the star's **Luminosity (L)**.

## Top Rectangle: Calculate Luminosity (L)

Please input the following values to calculate the **Luminosity (L)**:

<form id="luminosity-form">
    <div style="border: 1px solid #ccc; padding: 20px; margin-bottom: 20px;">
        <h3>Calculate Luminosity (L)</h3>
        
        <label for="m_top">Mass (M):</label>
        <input type="number" id="m_top" name="m_top" step="any" required>
        <br><br>

        <label for="x_top">Hydrogen Mass Fraction (X):</label>
        <input type="number" id="x_top" name="x_top" step="any" required>
        <br><br>

        <label for="z_top">Metallicity (Z):</label>
        <input type="number" id="z_top" name="z_top" step="any" required>
        <br><br>

        <button type="button" id="calculate-luminosity">Calculate Luminosity (L)</button>
    </div>
</form>

### Results for Luminosity Calculation:
<div id="luminosity-output">
    <p>Results will appear here.</p>
</div>

## Bottom Rectangle: Calculate Masses (M)

Please input the following values to calculate **Masses (M)**:

<form id="mass-form">
    <div style="border: 1px solid #ccc; padding: 20px;">
        <h3>Calculate Masses (M)</h3>

        <label for="l_bottom">Luminosity (L):</label>
        <input type="number" id="l_bottom" name="l_bottom" step="any" required>
        <br><br>

        <label for="x_bottom">Hydrogen Mass Fraction (X):</label>
        <input type="number" id="x_bottom" name="x_bottom" step="any" required>
        <br><br>

        <label for="z_bottom">Metallicity (Z):</label>
        <input type="number" id="z_bottom" name="z_bottom" step="any" required>
        <br><br>

        <button type="button" id="calculate-masses">Calculate Masses (M)</button>
    </div>
</form>

### Results for Mass Calculation:
<div id="mass-output">
    <p>Results will appear here.</p>
</div>

<script>
    // Wait for the DOM to load completely before attaching event listeners
    document.addEventListener("DOMContentLoaded", function() {
        // Function to calculate Luminosity (L)
        document.getElementById('calculate-luminosity').addEventListener('click', function() {
            const m = parseFloat(document.getElementById('m_top').value);
            const x = parseFloat(document.getElementById('x_top').value);
            const z = parseFloat(document.getElementById('z_top').value);

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
                let output = document.getElementById('luminosity-output');
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

        // Function to calculate Masses (M)
        document.getElementById('calculate-masses').addEventListener('click', function() {
            const l = parseFloat(document.getElementById('l_bottom').value);
            const x = parseFloat(document.getElementById('x_bottom').value);
            const z = parseFloat(document.getElementById('z_bottom').value);

            if (!l || !x || !z) {
                alert('Please enter Luminosity (L), Hydrogen Mass Fraction (X), and Metallicity (Z).');
                return;
            }

            const data = {
                "choice": "2",
                "Z": z,
                "L": l,
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
                let output = document.getElementById('mass-output');
                if (data.Pure_He_Mass) {
                    output.innerHTML = `
                        <p><strong>M_min:</strong> ${data.M_min}</p>
                        <p><strong>M_max:</strong> ${data.M_max}</p>
                        <p><strong>Pure_He_Mass:</strong> ${data.Pure_He_Mass}</p>
                    `;
                } else {
                    output.innerHTML = '<p style="color: red;">Error: Missing results</p>';
                }
            })
            .catch(error => {
                document.getElementById('mass-output').innerHTML = '<p style="color: red;">Error: ' + error.message + '</p>';
            });
        });
    });
</script>
