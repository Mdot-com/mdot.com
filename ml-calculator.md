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
    width: 200px;
    padding: 5px;
    text-align: left;
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

#intro-text {
  font-size: 1.1em;
  max-width: 1200px;
  margin: 0 auto 30px auto;
  text-align: justify;
}

</style>

<div id="intro-text">
  <p>
    Welcome to the mass-luminosity (ML) calculator page. Here we provide a web interface version for calculating minimum, maximum and pure-He mass-luminosity fit relations based on the stellar structure models in Sabhahit et al. (2025b). The novelty of this work is in the inclusion of stripped stars that can seemingly break simple homology relations due to the disproportionate contribution of H shell to the total luminosity.
  </p>

  <p><strong>MLRs in the literature</strong></p>
  <p>
    Typically, based on homology relations \(L \sim \mu^4 M^3\), the minimum luminosity for a given total mass \(M_\mathrm{tot}\) and surface hydrogen mass fraction \(X_\mathrm{H}\) occurs for a fully chemically homogeneous star with \(X(m) = X_\mathrm{H}\). The maximum luminosity occurs for a pure-He star configuration with \(X(m) = 0\) corresponding to full stripping. Vice versa, the maximum mass for a given luminosity and surface \(X_\mathrm{H}\) occurs for a chemically homogeneous star while the minimum mass occurs for a pure-He star. Such maximum and minimum MLRs are provided in Gräfener et al. (2011).
  </p>

  <p><strong>How do we build on it?</strong></p>
  <p>
    In Sabhahit et al. (2025b), we build a large grid of structure models.
  </p>
</div>


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
