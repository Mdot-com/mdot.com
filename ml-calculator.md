---
layout: default
title: ML Calculator
nav_order: 3
---

{% raw %}
<div style="max-width: 600px; margin: 2rem auto; padding: 1rem; text-align: center;">

  <h2 style="margin-bottom: 2rem;">
    Minimum, maximum and pure He luminosity for input Mass and surface Hydrogen
  </h2>

  <!-- Top Section: Luminosity -->
  <div style="border: 1px solid #ccc; padding: 1rem; border-radius: 10px; margin-bottom: 2rem; text-align: left;">
    <h3>Compute Luminosity from Mass</h3>
    <input type="number" id="massInput" placeholder="Mass (M)" style="width: 100%; padding: 0.5rem; margin-top: 1rem;">
    <input type="number" id="hydrogenInput1" placeholder="Hydrogen (X)" style="width: 100%; padding: 0.5rem; margin-top: 1rem;">
    <label><input type="checkbox" id="smcCheckbox1"> Use SMC metallicity</label>
    <button onclick="getLuminosity()" style="width: 100%; margin-top: 1rem; padding: 0.5rem;">Compute Luminosity</button>
    <p id="luminosityResult" style="margin-top: 1rem; font-weight: bold;"></p>
  </div>

  <!-- Bottom Section: Mass -->
  <div style="border: 1px solid #ccc; padding: 1rem; border-radius: 10px; text-align: left;">
    <h3>Compute Mass from Luminosity</h3>
    <input type="number" id="luminosityInput" placeholder="Luminosity (L)" style="width: 100%; padding: 0.5rem; margin-top: 1rem;">
    <input type="number" id="hydrogenInput2" placeholder="Hydrogen (X)" style="width: 100%; padding: 0.5rem; margin-top: 1rem;">
    <label><input type="checkbox" id="smcCheckbox2"> Use SMC metallicity</label>
    <button onclick="getMass()" style="width: 100%; margin-top: 1rem; padding: 0.5rem;">Compute Mass</button>
    <p id="massResult" style="margin-top: 1rem; font-weight: bold;"></p>
  </div>
</div>

<script>
  async function getLuminosity() {
    const m = parseFloat(document.getElementById('massInput').value);
    const x = parseFloat(document.getElementById('hydrogenInput1').value);
    const use_smc = document.getElementById('smcCheckbox1').checked;

    const response = await fetch("https://hkxx28fqq4.execute-api.eu-north-1.amazonaws.com/add", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ mode: "luminosity", m, x, use_smc })
    });

    const data = await response.json();
    document.getElementById('luminosityResult').innerText =
      `L_min: ${data.L_min?.toFixed(5)}, L_max: ${data.L_max?.toFixed(5)}, Pure He L: ${data.L_pure_He?.toFixed(5)}`;
  }

  async function getMass() {
    const L = parseFloat(document.getElementById('luminosityInput').value);
    const x = parseFloat(document.getElementById('hydrogenInput2').value);
    const use_smc = document.getElementById('smcCheckbox2').checked;

    const response = await fetch("https://hkxx28fqq4.execute-api.eu-north-1.amazonaws.com/add", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ mode: "mass", L, x, use_smc })
    });

    const data = await response.json();
    document.getElementById('massResult').innerText =
      `M_min: ${data.M_min}, M_max: ${data.M_max}, Pure He M: ${data.M_pure_He}`;
  }
</script>
{% endraw %}
