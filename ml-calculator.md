---
layout: default
title: ML Calculator
nav_order: 3
---

<!-- Load KaTeX -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.8/dist/katex.min.css">
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.8/dist/katex.min.js"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.8/dist/contrib/auto-render.min.js"
        onload="renderMathInElement(document.body, {delimiters: [{left: '\\(', right: '\\)', display: false}]});">
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
    
    <input type="number" id="ZInput1" placeholder="Metallicity (Z)" style="width: 100%; padding: 0.5rem; margin-top: 1rem;">

    <button onclick="getLuminosity()" style="width: 100%; margin-top: 1rem; padding: 0.5rem;">Compute Luminosity</button>
    <div id="luminosityResult" style="margin-top: 1rem; font-size: 1rem;"></div>
  </div>

  <!-- Bottom Section: Mass -->
  <div style="border: 1px solid #ccc; padding: 1rem; border-radius: 10px; text-align: left;">
    <h3>Compute Mass from Luminosity</h3>
    <input type="number" id="luminosityInput" placeholder="Luminosity (L)" style="width: 100%; padding: 0.5rem; margin-top: 1rem;">
    <input type="number" id="hydrogenInput2" placeholder="Hydrogen (X)" style="width: 100%; padding: 0.5rem; margin-top: 1rem;">
    
    <input type="number" id="ZInput2" placeholder="Metallicity (Z)" style="width: 100%; padding: 0.5rem; margin-top: 1rem;">

    <button onclick="getMass()" style="width: 100%; margin-top: 1rem; padding: 0.5rem;">Compute Mass</button>
    <div id="massResult" style="margin-top: 1rem; font-size: 1rem;"></div>
  </div>
</div>

<script>
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

    if (isNaN(m) || isNaN(x) || isNaN(Z)) {
      return alert("Please fill in all fields with valid numbers.");
    }

    try {
      const response = await fetch("https://nnv5wacde8.execute-api.eu-north-1.amazonaws.com/ML-calc", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ mode: "luminosity", m, x, Z })
      });

      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }

      const data = await response.json();
      console.log("Luminosity data:", data);  // Debugging line

      const latex = 
        "\\text{Minimum } \\log(L/L_\\odot):\\ " + data.L_min.toFixed(5) + "<br>" +
        "\\text{Maximum } \\log(L/L_\\odot):\\ " + data.L_max.toFixed(5) + "<br>" +
        "\\text{Pure He } \\log(L/L_\\odot):\\ " + data.L_pure_He.toFixed(5);

      renderLatex("luminosityResult", latex);
    } catch (error) {
      console.error("Error:", error);  // Debugging line
      alert("Error: " + error.message);
    }
  }

  async function getMass() {
    const L = parseFloat(document.getElementById('luminosityInput').value);
    const x = parseFloat(document.getElementById('hydrogenInput2').value);
    const Z = parseFloat(document.getElementById('ZInput2').value);

    if (isNaN(L) || isNaN(x) || isNaN(Z)) {
      return alert("Please fill in all fields with valid numbers.");
    }

    try {
      const response = await fetch("https://nnv5wacde8.execute-api.eu-north-1.amazonaws.com/ML-calc", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ mode: "mass", L, x, Z })
      });

      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }

      const data = await response.json();
      console.log("Mass data:", data);  // Debugging line

      const latex = 
        "\\text{Minimum mass } (M/M_\\odot):\\ " + data.M_min + "<br>" +
        "\\text{Maximum mass } (M/M_\\odot):\\ " + data.M_max + "<br>" +
        "\\text{Pure He mass } (M/M_\\odot):\\ " + data.M_pure_He;

      renderLatex("massResult", latex);
    } catch (error) {
      console.error("Error:", error);  // Debugging line
      alert("Error: " + error.message);
    }
  }
</script>
