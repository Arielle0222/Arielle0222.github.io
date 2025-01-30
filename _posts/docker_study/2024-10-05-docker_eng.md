---
layout: post
title:  "🐳 Docker Study 2nd [ENG]"
series: "docker"  # Docker series categorization
subtitle: "Building a Cloud-Based Docker Environment on Azure"
date:   2024-10-05
author: Arielle
categories: [code-road]
tags:   Docker Autonomous_Driving NVIDIA Azure Cloud
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

<h2>1. Create a VM (Virtual Machine)</h2>

I created an Ubuntu virtual machine using Microsoft Azure. Depending on user preferences, different image versions can be selected. <em>(If you don't remember what an image is, I recommend revisiting the previous post! 😎)</em> I named my virtual machine ‘kaggle-linux-gpu-vm’ and selected the remaining options accordingly. 

<img src="{{ '/assets/p.05/p2.1.1.png' | relative_url }}" alt="Descriptive Alt Text" />

One key point: When you create a virtual machine, a resource group with the same name followed by `_group` is automatically generated. This is because multiple resources (network interfaces, disks, public IPs, virtual networks, etc.) are created alongside the VM and are grouped together for efficient management. If this concept seems unclear, don’t worry—it didn’t make sense to me at first either. For now, just understand that a **resource group is a way to manage, track, and delete all resources related to a VM at once.**

Azure uses a **pay-as-you-go** pricing model. 💸 Charges are based on how long you use the virtual machine, so setting a budget limit in advance is a wise choice! 

The key aspects to check are **Image** and **Size**. Azure offers a variety of images. I selected **‘NVIDIA GPU-Optimized VMI -v23.09.1-64 Gen2’**. You can browse more options using the ‘See all images’ button. Even among NVIDIA GPUs, multiple versions exist, so choose one that fits your needs.

For memory (RAM), I chose **‘Standard_NC4as_T4_v3’**, which comes with **4 vCPUs and 28GiB of RAM**.

<img src="{{ '/assets/p.05/p2.1.2.png' | relative_url }}" alt="Descriptive Alt Text" />

<h4>1.1 💡 Azure Spot: A Smart Way to Use Cloud Resources!</h4>

Azure Spot Instances allow you to use cloud resources more economically. But first, let’s define **what cloud computing is**.

<h4>1.2 ☁️ What is Cloud?</h4>

<p><strong style="color: SteelBlue;">Cloud computing enables users to rent servers, storage, databases, networking, and other IT resources on demand via the internet. Simply put, you can run applications without installing anything on your computer—everything happens online!</strong></p>

Azure Spot Instances let you use idle Azure resources at a lower cost through a bidding system. You set a **maximum price per hour**, and if your bid is higher than the market rate, your instance remains active. However, if another user bids higher, or Azure runs out of capacity, your VM **may be evicted (stopped or deleted).** 😅

There are two types of **Eviction Policies**:

| Type         | Policy            | Pros | Cons |
|-------------|----------------|------|------|
| **Capacity Only** | Based solely on Azure's available capacity | No price fluctuation worries | May stop suddenly if resources run out |
| **Price or Capacity** | Bids based on max price per hour | Control costs, adjust for price changes | Instance may stop if price is exceeded |
| **Eviction Policy: Stop / Deallocate** | VM stops but data is preserved | Can restart later | Takes time to restart |
| **Eviction Policy: Delete** | VM and data are permanently deleted | Maximum cost savings | Data is unrecoverable |

At first, I thought, *“Why would I want my VM to stop unexpectedly?”* But Azure Spot is designed to let users **borrow spare resources at a lower price**, benefiting both Azure and users. 

💡 **For short-term testing or temporary projects, Spot Instances can significantly reduce costs!**

<img src="{{ '/assets/p.05/p2.1.3.png' | relative_url }}" alt="Descriptive Alt Text" />

<h4>1.3 🛠️ How to Solve the vCPU Quota Exceeded Issue</h4>

When I tried creating a VM, I got this error: 

<em><strong>"Operation could not be completed as it results in exceeding approved Low-priority quota."</strong></em>

<img src="{{ '/assets/p.05/p2.2.png' | relative_url }}" alt="Descriptive Alt Text" />

The problem? I exceeded the **vCPU quota** for the Korea Central region. Azure sets a default quota (3 vCPUs), but you can request an increase. I requested **1 additional vCPU, totaling 4.**

<img src="{{ '/assets/p.05/p2.3.png' | relative_url }}" alt="Descriptive Alt Text" />

After 10 minutes, the request was approved, and my VM was successfully created! 🎉

---

<h2>2. Connect to VM (Virtual Machine)</h2>

Now, it's time to connect my VM to my local machine. I used **SSH** for this.

```sh
ssh -i ~/.ssh/id_rsa.pem username@ip_address
```

However, I encountered a **permission error:**

```sh
Permissions 0644 for 'kaggle-linux-gpu-vm_key.pem' are too open
```

To fix this, I changed the file permissions:

```sh
chmod 600 kaggle-linux-gpu-vm_key.pem
```

After that, SSH worked perfectly! ✅

---

<h4>2.1 🐳 Using GPU in Docker Containers</h4>

To check if Docker can access the GPU, I ran:

```sh
docker run --rm --gpus all ubuntu nvidia-smi
```

This confirmed that the Docker container could access the GPU. 🚀

<img src="{{ '/assets/p.05/p2.19.png' | relative_url }}" alt="Descriptive Alt Text" />

---

<h2>3. Connect to GUI via RDP (MAC)</h2>

I used **Microsoft Remote Desktop** to connect to my VM's GUI. 

<img src="{{ '/assets/p.05/p2.23.png' | relative_url }}" alt="Descriptive Alt Text" />

Additionally, I installed **Chrome and VS Code** in the virtual environment. 

---

### Reference
- [Inflearn Practical Docker](https://www.inflearn.com/course/%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%A0%84%EB%AC%B8%EA%B0%80-%EC%8B%A4%EC%9A%A9%EC%A0%81%EC%9D%B8-%EB%8F%84%EC%BB%A4/dashboard)
