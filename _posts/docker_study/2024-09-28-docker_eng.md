---
layout: post
title:  "🐳 Docker Study 1st [ENG]"
series: "docker"  # Docker series categorization
subtitle: "Why Docker Matters for Autonomous Driving?"
date:   2024-09-28
author: Arielle
categories: [code-road]
tags:   Docker Autonomous_Driving NVIDIA
#cover:  "/assets/header2.gif"
---

<p>
<em>Starting from this post, my team and I, who aspire to become autonomous driving developers, will study Docker for seven weeks and document our learning journey on this blog. Docker is a name I have often encountered throughout my computer science studies. Most people familiar with Docker immediately think of its iconic logo—a whale carrying multiple containers on its back. Many senior developers in the industry praise Docker, claiming it is an essential tool to learn. However, as someone who is not yet in the field, Docker still feels just like a whale to me. Nevertheless, this is a great opportunity to dive deeper into Docker, so let’s get started!</em>
</p>

<p>
<em>
The upcoming posts will summarize what I learn from the course <strong> "Inflearn Practical Docker: Building My Own Deep Learning Cloud with Docker"</strong>. In this first post, I will discuss why Docker is crucial for aspiring autonomous driving developers, what Docker actually does, and introduce some fundamental commands that we will frequently use in the coming weeks.
</em>
</p>

<br>

<img src="{{ '/assets/2024/Docker_logo.svg' | relative_url }}" alt="Descriptive Alt Text" />

<br>

---

## 1. Why Docker Might Be Essential for Autonomous Driving Developers!
<strong>#AI Development & Containerization Trends #Automated Labeling Pipeline #Scalability</strong>

Where is the word “trend” used the most? Fashion might come to mind first, but another industry just as sensitive to trends is IT. Over the past few decades, few industries have changed as rapidly as IT, and the automotive industry is no exception. Cars are no longer just four-wheeled machines made of metal. 

I recently read an article about the autonomous driving startup **[Ghost Autonomy](https://www.aitimes.com/news/articleView.html?idxno=155106)** implementing ChatGPT’s LMM (Large Multimodal Model) into self-driving technology. While many experts argue that LMMs are premature for autonomous driving, opinions remain divided. On a side note, when Elon Musk first announced his plans for space travel, skepticism outweighed optimism—but Tesla proved the doubters wrong. 

Returning to Docker, the key takeaway is that future cars will be AI-powered by default. Docker serves as a **containerization tool that allows developers to experiment with different AI models and architectures in an isolated, scalable manner.** Our first keyword is **[‘AI Development & Containerization Trends’](https://dev.to/docker/the-rise-of-ai-in-software-development-key-insights-from-the-2024-docker-ai-trends-report-22dh)**.

NVIDIA further explains how **[Docker enhances efficiency in automated labeling pipelines](https://developer.nvidia.com/blog/building-ai-infrastructure-with-dgx-a100-for-autonomous-vehicles/)** for autonomous vehicle AI models. Training these models requires handling massive datasets, and labeling this data can be time-consuming. Docker optimizes this process by containerizing workloads, reducing unnecessary system overhead, and improving processing speed.

<img src="{{ '/assets/p.28/p28.1.png' | relative_url }}" alt="Descriptive Alt Text" />

For example, autonomous vehicles rely on multiple sensors—LiDAR, radar, and cameras—to collect data. Each sensor type has different formats and characteristics. Here, **Docker’s scalability and deployment features** shine. 

For instance, you can have **separate containers for processing LiDAR, camera, and radar data.** Even though each dataset requires different processing methods, Docker ensures that all these operations run in a unified environment. This eliminates the need for developers to manually manage each dataset and reduces conflicts in the development environment.

Furthermore, **Docker enables parallel data processing**, significantly increasing efficiency. Traditionally, handling different sensor data in a physical environment could be slow and error-prone. Docker solves this by providing a scalable and flexible infrastructure, greatly enhancing the efficiency of autonomous driving systems.

Even from this brief introduction, it’s clear that Docker is a powerful tool. Beyond AI containerization, automated labeling pipelines, and scalability, there are many more advantages to explore—which we will discover through hands-on practice in the coming weeks.

---

## 2. 10 Essential Docker Commands

To work with Docker efficiently, you need to master a few basic commands. When I first learned Git, I struggled with `commit`, `pull`, and `push` commands, but over time, they became second nature. Similarly, Docker commands might seem unfamiliar at first, but they follow a logical pattern.

Here are 10 essential Docker commands:

- docker pull
- docker build <<- Dockerfile
- docker run
- docker images
- docker ps (-a)
- docker stop <container name>
- docker start <container name>
- docker exec -it <container name> /bin/bash
- docker rm <container name>
- docker rmi <image name>

When I first encountered `images` and `containers` in Docker, I was confused. **At first, I thought ‘images’ referred to actual pictures.** But in Docker, an image is more like a **template or blueprint** for containers—similar to a mold for making fish-shaped pastries. 

If an **image is the pastry mold, then the container is the pastry shop where you make and sell them.** This analogy helped me understand Docker’s structure more intuitively.

<img src="{{ '/assets/p.28/p28.2.png' | relative_url }}" alt="Descriptive Alt Text" />

---

## 3. Try Using the Basic Commands

💻 `$ docker ps` & `$ docker ps -a`
These commands list running containers. Think of it as checking which pastry shops are currently open and selling fish-shaped pastries.

<img src="{{ '/assets/p.28/p28.3.png' | relative_url }}" alt="Descriptive Alt Text" />

💻 `$ docker images`
This command lists all available images (i.e., your collection of pastry molds).

<img src="{{ '/assets/p.28/p28.5.png' | relative_url }}" alt="Descriptive Alt Text" />

💻 `$ docker pull <image name>`
If an image (mold) is missing, you can pull it from Docker Hub.

<img src="{{ '/assets/p.28/p28.6.png' | relative_url }}" alt="Descriptive Alt Text" />

💻 `$ docker run -it --name <container name> <image>`
This command creates and runs a new container from an image.

<img src="{{ '/assets/p.28/p28.8.png' | relative_url }}" alt="Descriptive Alt Text" />

 💻 `$ docker stop <container name>` & `$ docker rm <container name>`
To delete an image, you must first stop and remove the container using it.

<img src="{{ '/assets/p.28/p28.12.png' | relative_url }}" alt="Descriptive Alt Text" />

---

<p>
<em>
This wraps up my first Docker study post! As a beginner, my explanations might seem oversimplified, but I believe this step-by-step approach will help solidify my understanding. Next time, I will dive into setting up an Ubuntu virtual machine on Azure to deepen my Docker knowledge. Stay tuned!
</em>
</p>

---

### Reference
- [Docker Cheatsheet](https://docs.docker.com/get-started/docker_cheatsheet.pdf)
