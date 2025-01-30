---
layout: page
title: About
permalink: /about/
main_nav: true
nav_order: 1
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>About</title>
    <style>
        .content {
            max-width: 800px;
            margin: auto;
            font-family: Arial, sans-serif;
            line-height: 1.6;
            text-align: left;
        }

        .image-container {
            text-align: center;
            margin: 20px 0;
        }

        .image-container img {
            width: 50%;
            height: auto;
        }

        .section-container {
            display: flex;
            align-items: center;
            justify-content: space-between;
            text-align: left;
            background: #f9f9f9;
            padding: 20px;
            border-radius: 10px;
            margin: 30px 0;
        }

        .section-text {
            flex: 1;
        }

        h2 {
            color: #333;
        }

        h4.center-title {
            color: steelblue; /* 제목 컬러 변경 */
            text-align: left; /* 제목을 가운데 정렬 */
            font-size: 1.3em;
            margin-top: 30px;
        }

        strong {
            color: #000;
        }

        /* Responsive Design */
        @media (max-width: 768px) {
            .section-container {
                flex-direction: column;
                text-align: center;
            }
            .section-container img {
                margin: 0 auto 15px auto;
            }
        }

    </style>
</head>
<body>

<div class="content">

    <!-- Logo Placement -->
    <div class="image-container">
        <img src="/assets/logo_r.png" alt="Logo" />
    </div>

    <h2>Hi all! I'm Arielle 👋🏻</h2>

    <p>
        Hello! I’m <strong>SEOHEE KIM (ARIELLE)</strong>, a student majoring in 
        <strong>Software and Artificial Intelligence (SW/AI)</strong> in the Republic of Korea. 🇰🇷
    </p>

    <p>
        "The future of mobility isn't just about software — <strong>it’s about the perfect synergy between software and hardware.</strong>"
    </p>

    <p>
        As a self-driven autonomous vehicle developer, I'm specializing in 
        <strong>computer vision, deep learning, and software engineering for self-driving cars.</strong> 
        But I believe that <strong>understanding software alone isn’t enough to truly shape the future of mobility.</strong> 
        That’s why I’m also delving into automotive mechanics, studying F1 and automotive hardware.
    </p>

    <br>

    <!-- Centered Title -->
    <h4 class="center-title">💻 What You'll Find in This Blog</h4>

    <!-- Code & Road Section -->
    <div class="section-container">
        <div class="section-text">
            <strong>🚗 Code & Road</strong>
            <p>
                Here, I will <strong>organize all my technical studies related to autonomous driving.</strong> 
                This includes not only my past projects but also cloud computing, Docker, AI, 
                and other essential technologies for self-driving cars.
            </p>
        </div>
    </div>

    <!-- Story Garage Section -->
    <div class="section-container">
        <div class="section-text">
            <strong>🔍 Story Garage</strong>
            <p>
                I will document <strong>everything about the mobility ecosystem, including autonomous driving.</strong> 
                Beyond just code and technology, I will also <strong>share my personal journey in this field.</strong>
            </p>
        </div>
    </div>

    <!-- Centered Title -->
    <h4 class="center-title">🚀 My Vision</h4>

    <p>
        My goal is to <strong>bridge the gap between software and hardware in the autonomous driving industry</strong>, 
        ensuring that software-driven vehicles aren’t just intelligent but also reliable, safe, and well-maintained.
    </p>

    <p>
        I want to contribute to a future where <strong>mobility is seamless, autonomous, and free from accidents</strong> — 
        and I’m determined to make that vision a reality through both code and real-world vehicle expertise.
    </p>

    <p><strong>Welcome to my journey at the intersection of AI and mechanics!</strong>
    <br>Thank you for stopping by, and have a great day! 😀</p>

</div>

</body>
</html>
