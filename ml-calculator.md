---
layout: default
title: ML Calculator
nav_order: 3
---

<h1>ML Calculator</h1>

<div style="max-width: 500px; margin: 2rem auto; padding: 1rem; border: 1px solid #ccc; border-radius: 8px;">
  <p>Enter two numbers to add them:</p>

  <input type="number" id="num1" placeholder="First number" style="width: 100%; padding: 0.5rem; margin-bottom: 1rem;">

  <input type="number" id="num2" placeholder="Second number" style="width: 100%; padding: 0.5rem; margin-bottom: 1rem;">

  <button onclick="addNumbers()" style="width: 100%; padding: 0.5rem;">Add</button>

  <p id="result" style="margin-top: 1rem; font-weight: bold;"></p>
</div>

<script>
  async function addNumbers() {
    const a = parseFloat(document.getElementById('num1').value);
    const b = parseFloat(document.getElementById('num2').value);

    const response = await fetch('https://hkxx28fqq4.execute-api.eu-north-1.amazonaws.com/add', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ a: a, b: b })
    });

    const data = await response.json();
    document.getElementById('result').innerText = "Sum: " + data.sum;
  }
</script>
