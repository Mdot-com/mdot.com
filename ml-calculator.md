<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML"></script>

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
    const resultText = `
      Minimum log(L/L_\\odot): ${data.L_min.toFixed(5)}<br>
      Maximum log(L/L_\\odot): ${data.L_max.toFixed(5)}<br>
      Pure He log(L/L_\\odot): ${data.L_pure_He.toFixed(5)}
    `;
    document.getElementById('luminosityResult').innerHTML = resultText;
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
    const resultText = `
      Minimum Mass: ${data.M_min.toFixed(5)}<br>
      Maximum Mass: ${data.M_max.toFixed(5)}<br>
      Pure He Mass: ${data.M_pure_He.toFixed(5)}
    `;
    document.getElementById('massResult').innerHTML = resultText;
    MathJax.Hub.Queue(["Typeset", MathJax.Hub, 'massResult']);
  }
</script>
