---
layout: post
title:  "🐳 Docker Study 3rd [ENG]"
series: "docker"  # Docker series categorization
subtitle: "Optimizing Kaggle Python Docker for Data Analysis"
date:   2024-10-11
author: Arielle
categories: [code-road]
tags:   Docker Autonomous_Driving NVIDIA Azure Cloud Python
#cover:  "/assets/instacode.png"
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Docker Study</title>
    <style>
        h2, h4 {
            color: SteelBlue;
        }
    </style>
</head>
<body>
</body>
</html>

## 1. Setting up a Python Analysis Environment

Kaggle provides a **‘Kaggle GPU Image’**, a Docker image optimized for GPU-based machine learning. This image includes popular deep learning frameworks like **TensorFlow and PyTorch** along with essential libraries, making it convenient for AI development. By default, Kaggle GPU is based on **Tesla T4 GPU** hardware, which we will verify using code later.

To begin, search **‘kaggle python docker image’** on GitHub, where you can find the repository. The README file allows you to choose between **CPU-only** and **GPU** versions. From experience, selecting **GPU** is a better long-term choice for flexibility, especially if using **Dev Container Extension**, which generates `.json` config files for further customization.

<img src="{{ '/assets/p.11/p11.0.png' | relative_url }}" alt="Descriptive Alt Text" />

Clicking on **GPU** leads to the following directory, where Kaggle organizes its GPU images by version. As of **October 11, 2024**, the latest available version is **v153**, which I pulled.

<img src="{{ '/assets/p.11/p11.1.png' | relative_url }}" alt="Descriptive Alt Text" />

### 💻 Related Commands

<h4>💻 Related Commands</h4>

✅ <code>ssh -i <~/.ssh/id_rsa.pem> <username>@<ip주소></code>

<strong style="color: SteelBlue;"> - Connecting Virtual Machine Server</strong>

✅ <code>sudo usermod -aG docker <username></code>

<strong style="color: SteelBlue;"> - Add a specific user to the Docker group on this system</strong>

<strong style="color: SteelBlue;"> - The usermod command modifies user attributes, and the -aG option adds the user to a new group. (In a team project, this command can be used to add a teammate to the container.)</strong>

✅ <code>docker pull <image_names><version></code>

<strong style="color: SteelBlue;"> - Download a Docker image from Docker Hub or another image repository!</strong>


---

<img src="{{ '/assets/p.11/p11.2.png' | relative_url }}" alt="Descriptive Alt Text" />

---

## 2. Setting Up a Remote Development Environment
### 2.1 Connecting to a VM via Remote Method

Once connected to the virtual environment, I also used **Remote Connection** via a new window. [(See ‘Docker Study 2nd’)](http://localhost:4000/code%20&%20road/2024/10/05/docker.html)

#### 🛜 Connection Steps:
**Remote Window → Connect to Tunnel → GitHub → kaggle-linux-gpu-vm → Create ‘kaggle-python-gpu-env’ Folder → Generate ‘Dockerfile’ (Docker Intelligence should be auto-applied!)**

Inside **Dockerfile**, I set the base image:

```dockerfile
FROM gcr.io/kaggle-gpu-images/python:v153

# Install additional dependencies
RUN pip install yfinance
```

To verify, I ran:

```sh
sudo docker images
```

This confirmed that `v153` was already available in my system.

<img src="{{ '/assets/p.11/p11.3.png' | relative_url }}" alt="Descriptive Alt Text" />

### 2.2 Setting Up Dev Container Extension

Ensure **Dev Container Extension** and **Remote Tunnel Extension** are installed on both the VM and local machine.

```sh
# Change directory to the project folder
cd kaggle-python-gpu-env
```

Then, select **‘Add Dev Container Configuration Files…’** under **Editing on kaggle-linux-gpu-vm (bottom left remote button)**.

<img src="{{ '/assets/p.11/p11.4.png' | relative_url }}" alt="Descriptive Alt Text" />

Choose **‘From Dockerfile’**.

<img src="{{ '/assets/p.11/p11.5.png' | relative_url }}" alt="Descriptive Alt Text" />

Click OK to complete the setup.

<img src="{{ '/assets/p.11/p11.6.png' | relative_url }}" alt="Descriptive Alt Text" />

---

### 2.3 Differences Between Dockerfile and devcontainer.json

When using **Dev Container Extension**, a `.json` file is automatically generated. But how is this different from `Dockerfile`?

#### 📂 Dockerfile

```dockerfile
FROM python:3.9
WORKDIR /app
COPY . /app
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```

**Dockerfile is like a recipe that defines the environment needed to run an application.** It sets up:

- **Base image**
- **Dependencies** (e.g., libraries, packages)
- **Execution commands** (what runs when the container starts)

#### 📂 devcontainer.json

```json
{
  "name": "Python Dev Container",
  "dockerFile": "./Dockerfile",
  "settings": {
    "python.pythonPath": "/usr/local/bin/python"
  },
  "extensions": [
    "ms-python.python",
    "ms-toolsai.jupyter"
  ],
  "forwardPorts": [5000],
  "postCreateCommand": "pip install -r requirements.txt"
}
```

**devcontainer.json is used to configure the development environment, not the application runtime.** It handles:

- **Editor settings** (e.g., VS Code extensions)
- **Port forwarding**
- **Post-setup commands**

#### 📌 Summary Table

| Feature  | Dockerfile  | devcontainer.json  |
|----------|------------|--------------------|
| Purpose  | Defines the runtime environment  | Configures development settings |
| Usage  | Used in production, testing, and development  | Mainly for development in VS Code |
| Settings  | Base image, dependencies, execution commands  | Extensions, ports, additional commands |

**Most of the time, devcontainer.json references Dockerfile**, similar to how a **storefront’s layout might change, but the core product (Docker image) remains the same**.

To add extensions, go to VS Code **Extensions → Copy Extension ID → Paste in devcontainer.json**.

<img src="{{ '/assets/p.11/p11.8.png' | relative_url }}" alt="Descriptive Alt Text" />

---

### 🔍 Key Takeaways
- **Dockerfile** defines the runtime environment.
- **devcontainer.json** customizes the development experience.
- Dev Containers help streamline Python data analysis with Kaggle GPU Docker.

*For further insights, see my upcoming benchmark comparison post!* 🚀

---

### Reference
- [Inflearn Practical Docker](https://www.inflearn.com/course/%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%A0%84%EB%AC%B8%EA%B0%80-%EC%8B%A4%EC%9A%A9%EC%A0%81%EC%9D%B8-%EB%8F%84%EC%BB%A4/dashboard)
