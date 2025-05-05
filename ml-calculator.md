---
layout: default
title: ML Calculator
nav_order: 3
---

{% raw %}
<!-- Include MathJax for LaTeX rendering -->
<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

<div style="max-width: 600px; margin: 2rem auto; padding: 1rem; text-align: center;">

  <h2 style="margin-bottom: 2rem;">
    Minimum, maximum and pure He mass-luminosity relations
  </h2>

  <!-- Top Section: Luminosity -->
  <div style="border: 1px solid #ccc; padding: 1rem; border-radius: 10px; margin-bottom: 2rem; text-align: left;">
    <h3>Compute Luminosity from Mass</h3>
    <input type="number" id="massInput" placeholder="Mass (M)" style="width: 100%; padding: 0.5rem; margin-top: 1rem;">
    <input type="number" id="hydrogenInput1" placeholder="Hydrogen (X)" style="width: 100%; padding: 0.5rem; margin-top: 1rem;">
    
    <label for="smcDropdown1">Select Metallicity:</label>
    <select id="smcDropdown1" style="width: 100%; padding: 0.5rem; margin-top: 1rem;">
      <option value="false">LMC</option>
      <option value="true">SMC</option>
    </select>

    <button onclick="getLuminosity()" style="width: 100%; margin-top: 1rem; padding: 0.5rem;">Compute Luminosity</button>
    <p id="luminosityResult" style="margin-top: 1rem; font-size: 1rem;"></p>
  </div>

  <!-- Bottom Section: Mass -->
  <div style="border: 1px solid #ccc; padding: 1rem; border-radius: 10px; text-align: left;">
    <h3>Compute Mass from Luminosity</h3>
    <input type="number" id="luminosityInput" placeholder="Luminosity (L)" style="width: 100%; padding: 0.5rem; margin-top: 1rem;">
    <input type="number" id="hydrogenInput2" placeholder="Hydrogen (X)" style="width: 100%; padding: 0.5rem; margin-top: 1rem;">
    
    <label for="smcDropdown2">Select Metallicity:</label>
    <select id="smcDropdown2" style="width: 100%; padding: 0.5rem; margin-top: 1rem;">
      <option value="false">LMC</option>
      <option value="true">SMC</option>
    </select>

    <button onclick="getMass()" style="width: 100%; margin-top: 1rem; padding: 0.5rem;">Compute Mass</button>
    <p id="massResult" style="margin-top: 1rem; font-size: 1rem;"></p>
  </div>
</div>

<script>
  async function getLuminosity() {
    const m = parseFloat(document.getElementById('massInput').value);
    const x = parseFloat(document.getElementById('hydrogenInput1').value);
    const use_smc = document.getElementById('smcDropdown1').value === "true";

    const response = await fetch("https://nnv5wacde8.execute-api.eu-north-1.amazonaws.com/ML-calc", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ mode: "luminosity", m, x, use_smc })
    });

    const data = await response.json();
    document.getElementById('luminosityResult').innerHTML =
      `\\( \\text{Minimum log(L/L_\\odot)}: \\) ${data.L_min.toFixed(5)}<br>
       \\( \\text{Maximum log(L/L_\\odot)}: \\) ${data.L_max.toFixed(5)}<br>
       \\( \\text{Pure He log(L/L_\\odot)}: \\) ${data.L_pure_He.toFixed(5)}`;

    MathJax.Hub.Queue(["Typeset", MathJax.Hub, 'luminosityResult']);
  }

  async function getMass() {
    const L = parseFloat(document.getElementById('luminosityInput').value);
    const x = parseFloat(document.getElementById('hydrogenInput2').value);
    const use_smc = document.getElementById('smcDropdown2').value === "true";

    const response = await fetch("https://nnv5wacde8.execute-api.eu-north-1.amazonaws.com/ML-calc", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ mode: "mass", L, x, use_smc })
    });

    const data = await response.json();
    document.getElementById('massResult').innerHTML =
      `\\( \\text{Minimum mass (M/M_\\odot)}: \\) ${data.M_min}<br>
       \\( \\text{Maximum mass (M/M_\\odot)}: \\) ${data.M_max}<br>
       \\( \\text{Pure He mass (M/M_\\odot)}: \\) ${data.M_pure_He}`;

    MathJax.Hub.Queue(["Typeset", MathJax.Hub, 'massResult']);
  }
</script>

{% endraw %}
