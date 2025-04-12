---
layout: archive
title: ""
author_profile: true
sidebar:
---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Your GitHub Page</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            height: 100vh;
            overflow: hidden;
            font-family: Arial, sans-serif;
        }

         .bg-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            animation: slide 40s infinite;
        }

        .bg-container img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        @keyframes slide {
            0% { background-image: url('assets/images/bg1.jpg'); }
            25% { background-image: url('assets/images/bg2.jpg'); }
            50% { background-image: url('assets/images/bg3.jpg'); }
            75% { background-image: url('assets/images/bg4.jpg'); }
            100% { background-image: url('assets/images/bg1.jpg'); }
        }
    </style>
</head>
<body>

<div class="bg-container"></div>

<p style="font-size: 1.1em; position: relative; z-index: 10;">Hello and welcome to my personal webpage! I am a stellar astrophysicist with a broad interest in massive stars. I have experience in stellar evolution modelling following the life and death of massive stars, as well as atmosphere modelling studying their strong stellar wind structure and mass-loss physics.</p>

Explore my:

- [Publications](/publications/)
- [CV](/files/cv.pdf)

</body>
</html>
