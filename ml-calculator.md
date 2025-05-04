---
layout: default
title: ML Calculator
nav_order: 3
---

<h1>ML Calculator</h1>

<p>Enter two numbers to add them using the AWS backend:</p>

<input type="number" id="num1" placeholder="First number">
<input type="number" id="num2" placeholder="Second number">
<button onclick="addNumbers()">Add</button>

<p id="result"></p>

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
