---
layout: post
title:  "🐳 Docker study 4th [ENG]"
series: "docker"  # Docker 시리즈에 대한 시리즈 지정
subtitle: "Running RStudio on Docker: Leveraging GPU for Data Analysis"
date:   2024-10-17
author: Arielle
categories: [code-road]
tags:   Docker Autonomous_Driving NVIDIA Azure Cloud
#cover:  "/assets/instacode.png"
---



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

<p>
<em>
In the previous <Docker Study 3rd> post, I set up a Python environment in a container, or virtual environment, using a Kaggle Image. For this 4th week, I conducted a hands-on session to build R, a widely used data analysis environment, using a Kaggle Image.

This time, I delved deeper into Dockerfile, but even though it was covered in week 3, applying it in a new context made my mind go completely blank again. (Maybe I was a goldfish in my past life...🤦‍♀️) 
</em>
</p>

<p>
<em>
Since I'm constantly encountering new concepts, I feel like I'm learning at a slower pace, but at least I have the determination to solve the challenges ahead, which is a relief! 💪

If you can’t summarize what a ‘Dockerfile’ is in a single sentence before diving into this post, I highly recommend quickly reviewing the 📂 Dockerfile? section from the previous post! 🏁
</em>
</p>
---
<h2>1. Setting up a R analysis environment</h2>
<h4>1.1 Installing a Kaggle GPU image for R</h4>

If you search for ['Kaggle gpu R image'](https://github.com/Kaggle/docker-rstats), you'll find a link to download an image that allows you to use R inside a container. Since the process is similar to building an image for the Kaggle Python environment, it was relatively straightforward. As of the installation date (October 14, 2024), the latest version of R is v59.

I installed it <code>docker pull gcr.io/kaggle-gpu-images/rstats:v59</code> on the virtual machine. 

<img src="{{ '/assets/p.14/p14.1.png' | relative_url }}" alt="Descriptive Alt Text" />

<h4>1.2 Running an R container in the RStudio Server IDE environment</h4>

So far, I have been using Docker by downloading the necessary images and running them in the container, essentially in a virtual environment terminal. This time, I attempted to run the container in an IDE environment (Web) using RStudio Server.

The image below shows the result of accessing the IDE through the R container, successfully running within a web interface.

<img src="{{ '/assets/p.14/p14.2.png' | relative_url }}" alt="Descriptive Alt Text" />

This can be achieved with just a single command. It's a bit long, but if you break it down step by step, it's not too difficult to understand.

✅ <strong>Rstudio Web Browser connect code</strong>  <code>docker run -d -p 8787:8787 --gpus all -v "/home/Arielle/rproject:/home/rstudio/rproject" --name rstudio-container gcr.io/kaggle-gpu-images/rstats:v59 /bin/bash -c "rstudio-server start && tail -f /dev/null"</code>


The command <code>docker run -d</code> is used to start a new container based on the specified image. The -d option enables "Detached mode", which allows the container to run in the background. This means that the terminal remains free for other tasks and is not locked to the container process.

If the -d option is not used, the terminal will remain attached to the container’s process, preventing you from executing any other commands while the container is running. Therefore, using -d is important when you need to run a container in the background and continue working on other tasks in the terminal!


The option <code>-p 8787:8787</code> is used to set up port forwarding between the host system and the container.

The first 8787 (before :) refers to the host system's port.
The second 8787 (after :) refers to the container's port, where the RStudio Server runs by default.
This configuration allows the RStudio Server inside the container to be accessible from your local system. As a result, you can open a web browser and access the RStudio Server by navigating to:

👉 http://localhost:8787

This way, the host system forwards requests from its port 8787 to the same port inside the container, enabling smooth interaction between the containerized R environment and your local browser. 


However, while explaining this in a simple sentence, I realized that I didn't have a clear understanding of ports. As I followed the lecture, I kept questioning whether I truly grasped the concept. (The only port numbers I was somewhat familiar with were 1004 and 22?? 🤨)

So, to never get confused about ports again, I decided to take a step back and properly organize my understanding of them! Let’s dive into it!

---
<h4>🚪 Port?</h4>


First of all, I didn't fully understand what ports were, but I had unknowingly been using them in various places. One representative example was when I set up a Mac virtual environment [using RDP](https://arielle0222.github.io/code%20&%20road/2024/10/05/docker.html) (check out the post here).

To make it clearer, I created a simple diagram that visually organizes my understanding of ports. See below!👇

<img src="{{ '/assets/p.14/p14.1.1.png' | relative_url }}" alt="Descriptive Alt Text" />


From the client-server perspective, as shown on the left side of the diagram, each service that the server provides is assigned a specific port number, which allows clients to connect to it.

For example:

- RDP (Remote Desktop Protocol) uses port 3389.
- SSH (Secure Shell) uses port 22.
- VNC (Virtual Network Computing) uses port 5900.

What about the Docker container perspective?
Now, let's shift our understanding to Docker containers. In this perspective:

The traditional client (a user or device) is replaced by the local host.
The traditional server is replaced by the container.
A Docker container can be thought of as an isolated, self-contained environment designed to run a specific application or service—similar to an independent computer.

One important takeaway from the diagram is that containers operate within a "User Space" on top of the host operating system. Inside this space, each containerized service (application image) runs separately, maintaining its own environment.

Even though containers share the same host machine, each container runs its application in isolation, with dedicated resources and ports to communicate with the outside world. This is why port forwarding (e.g., -p 8787:8787) is crucial in Docker, as it allows external access to the services running inside containers.

You might wonder why I suddenly started talking about container structures while discussing ports. Earlier, I mentioned that each service is assigned a unique port number. This means that the RStudio service I just downloaded must also have a unique port number. That number is 8787, as seen in the command <code>-p 8787:8787</code>!

However, you’ll notice that 8787 appears twice, separated by a colon (:). The first 8787 represents the port assigned to the localhost, while the second 8787 is the port assigned to the container. This is different from traditional client-server models, where only the server side is assigned a port number. Here, both sides are assigned a port number—this is the key difference!

Another interesting point is that the port number assigned to the localhost does not have to be 8787; it can be set to any number the user chooses.

This raises another question: When would you manually specify the local host port number?
The answer is when you run multiple RStudio Server containers on the same machine. In such cases, assigning different local ports to each container ensures flexibility (pliability).

However, the reason for setting the same port number for both the localhost and the container is not just for convenience but also for isolation. When multiple containers are running, having the same port number makes it easier to locate each service. This concept naturally leads to the discussion of isolation.

By default, a container operates in complete isolation from the external environment. However, when we specify a port pair, we establish communication between the localhost and the container. This allows the isolated container to become accessible from the outside, which is where Docker’s true power comes into play!

---

Now, let's return to the command for running a container on the web! After specifying the port, you’ll notice the option `--gpus all`. As the name suggests, this allows the container to access the local GPU resources. If your local host is equipped with a GPU, the container can utilize it for computation. However, if GPU access is not needed or your system doesn’t have a GPU, you can omit this option without any issue.


The part -v "/home/Arielle/rproject:/home/rstudio/rproject" represents the concept of Volume Mounting in Docker. In simple terms, volume mounting is a feature that links a file or directory from the host system to a specific location inside the container.

Here, we are pairing:

- Host directory: `/home/Arielle/rproject`
- Container directory: `/home/rstudio/rproject`

This setup allows for seamless file sharing between the host and the container. You can:

- Use files created on your local machine inside the container
- Access files generated inside the container directly from the host

For example, if you run the command mkdir testfolder on your local machine, you can immediately see the newly created 'testfolder' inside the container—as shown in the image below (Container with WEB)

<img src="{{ '/assets/p.14/p14.8.png' | relative_url }}" alt="Descriptive Alt Text" />

The option `--name rstudio-container` is used to assign a name to the created container, and here, it is specified as `rstudio-container`.

The last part,`gcr.io/kaggle-gpu-images/rstats:v59, represents the Docker image (RStudio), indicating its repository path and version.

---

<h2>2. How to work Python package in R?</h2>

If you have successfully connected to your local host through the container, you will see the R program interface as shown below. Since I had never worked with R before, I wasn’t familiar with its syntax. The command `reticulate::py_config()` is a function in R that sets up and displays information about the Python environment.
Interestingly, R can integrate with Python using the `reticulate package`. When I ran the command, I noticed the following message appeared in the console:

<em>Would you like to create a default Python environment for the reticulate package? (Yes/no/cancel)</em>

This message indicated that there was no existing Python environment linked to R, and the reticulate package was prompting me to install a new one. So, I simply selected "YES" to install Python and proceed with the setup.

<img src="{{ '/assets/p.14/p14.5.png' | relative_url }}" alt="Descriptive Alt Text" />

For reference, while I was able to visually confirm RStudio using the RStudio IDE, this time, I checked it through the container’s terminal mode. (After all, it’s good to learn both GUI and terminal approaches!) First, I ran `docker ps` to check the name of the running container (rstudio-container). 

Then, I used the following command to directly access RStudio within the container. `docker exec -it rstudio-container R`
As shown in the image below, R ran successfully within the container’s terminal, confirming that it works properly even without a graphical interface.

✅  `docker exec -it rstudio-container R`  여기서 `-it` 는 `-i` (interactive), `-t` (tty)가 합쳐진 명령어로 컨테이너 내부에서 명령어를 실행할 수 있게 해주면서 동시에 터미널에서 입력한 명령어와 출력을 보기 위해 화면을 터미널처럼 만들어주는 기능을 한다! (참고로 해당 터미널에서 exit하고 싶으면 `q()` 로 나오면 된다!)

<img src="{{ '/assets/p.14/p14.6.png' | relative_url }}" alt="Descriptive Alt Text" />

아까 RStudio(WEB)에서 Python을 설치해 주었다. 설치 여부를 물어보길래 얼래벌래 설치를 하기는 했는데 어디에 설치 되었는지 한번 쯤은 확인할 필요가 있을 것 같다. `reticulate::py_config()` 로 터미널에서 확인해보니 `/root/.local/share/r-minicconda/env/r-reticulate/bin/python` 경로에 위치해 있음을 확인했다. 그런데 지금 보면 `r-miniconda` 라는 폴더의 하위 경로에 python이 설치되어 있는 걸로 보이는데, 나는 해당 폴더를 생성한 적이 분명 없다. 

이를 제대로 이해하기 위해서는 Docker 컨테이너 내부에서 RStudio와 Python 환경을 어떻게 연결하고 상호작용하는지 이해할 필요가 있다.

<img src="{{ '/assets/p.14/p14.7.png' | relative_url }}" alt="Descriptive Alt Text" />

<img src="{{ '/assets/p.14/p14.7.1.png' | relative_url }}" alt="Descriptive Alt Text" />

바로 위 그림으로 이해해보도록 하자! 파이썬 패키지를 이용해 Keras, Tensorflow가 R에서 어떻게 작동하는지 플로우를 그린 그림으로 총 5단계로 진행 된다. 

1️⃣ R에서 딥러닝을 수행하려면 R Keras 패키지를 사용해야 한다. 하지만, R 자체에는 Keras나 TensorFlow가 내장되어 있지 않기 때문에 Python 패키지를 불러와 사용해야 한다.

2️⃣ 자 이제 Python 패키지가 필요함을 알았으니 R로 가져와야 하는데 특이하게 R에서는 `reticulate` 라는 패키지가 필요하다. 그래서 `/root/.local/share/r-minicconda/env/r-reticulate/bin/python` 이 경로처럼 `r-reticulate` 가 python의 상위 경로에 있었던 것이다!

3️⃣ 현재 Python 패키지의 경우 reticulate가 사용하는 Python 환경으로 들어오게 된다. 이게 무슨 말이라면 만약 `reticulate`가 miniconda라는 가상 환경을 사용하고 있다면, Python 패키지는 miniconda 환경 안에 설치되어 `miniconda`가 Python 패키지를 저장하고 관리하는 공간이 되는 것이다. 반면에 만약 `reticulate`가 다른 Python 환경을 사용 중이라면,그 환경에 Python 패키지가 설치된다는 뜻이다! 예를 들어 시스템에 미리 설치된 Python이 있다면 그 환경으로 Python 패키지가 설치될 수도 있다. 

아무튼 포인트는 `reticulate`가 사용하는 Python 환경에 따라 Python 패키지가 설치되는 위치가 달라진다는 것이다!

4️⃣ 위의 단계가 정상적으로 진행되면 Python을 위해 keras, tensorflow를 사용할 수 있게 된다. keras는 RStudio에서 알고리즘을 생성할 수 있게 된다.(UI)

5️⃣ 이후 Tensorflow는 Keras의 백엔드로 작동하며  실질적인 연산을 하게 된다.

---

<h2>3. Custom Dockerfile</h2>

앞서 Docker study 3rd에서 Dockerfile에 대해서 알아봤었다. 그때는 붕어빵을 예로 들면서 Dockerfile을 붕어빵 틀을 만드는 레시피 정도로 설명 했었는데, 이번에는 조금 더 소프트웨어 관점에서 생각해보고자 한다. 

---
📂 <strong>Dockerfile?</strong> ([’👉🏻Docker study 3rd'](https://arielle0222.github.io/code%20&%20road/2024/10/11/docker.html))

#**붕어빵 틀**을 만드는 레시피

Dockerfile을 컨테이너에서 실행할 환경을 만드는 레시피로 생각해 봤다(붕어빵🐟🍞 다시 등장). 우리가 어떤 프로그램을 실행하려면 그 프로그램이 돌아가게 할 환경이 필요하다. 예를 들어 프로그램이 Python으로 짜여 있으면 Python을 설치해야 하는 것처럼, Dockerfile은 그런 것들을 자동으로 설정해주는 파일이라고 이해하면 된다.

**A. 베이스 이미지 설정**: 붕어빵을 만들 때 어떤 맛의 반죽을 쓸지 정하는 개념으로 Python이나 Ubuntu 같은 기본 이미지를 설정한다!

**B. 필요한 프로그램 설치**: 붕어빵 반죽에 필요한 재료를 넣는 것과 같다! 프로그램이 실행되기 위해 필요한 **라이브러리나 패키지**를 설치할 수 있다는 뜻으로 예를 들어, 붕어빵 반죽에 초콜릿 칩을 넣듯, 프로그램에 필요한 라이브러리를 추가한다!

**C. 실행 명령어**: 붕어빵이 완성된 후에 어떻게 구울지 정하는 것처럼 컨테이너가 켜졌을 때 어떤 프로그램을 실행할지 설정할 수 있다! 예를 들어 붕어빵을 굽는 온도와 시간을 정할 수 있는 것처럼 프로그램 역시 자동으로 실행될 명령어를 설정할 수 있다!

---

Dockrfile은 우리가 해당 프로그램을 돌아가게 할 수 있는 기본 환경을 만들어주는 파일로 베이스 이미지를 설정하고, 필요한 패키지도 설정하고, 실행 명령어도 직접 설정할 수 있다고 배웠다. 아래 사진은 위에서 우리가 컨테이너 이미지로 사용한 Kaggle R 이미지의 Dockerfile이다. `RUN, ADD, ENV, ARG, LABEL, CMD` 같은 명령어들로 파일이 구성되어 있는 걸 확인할 수 있는데 이들의 기능을 하나씩 정리해보도록 하자!

<img src="{{ '/assets/p.14/p14.3.png' | relative_url }}" alt="Descriptive Alt Text" />

---

<h4>💻 Related Commands</h4>

✅ `ADD` : 로컬 호스트 내 파일이나 폴더를 Docker 이미지로 복사

- 로컬 파일을 컨테이너로 복사하거나 URL에서 파일을 가져올 수도 있고 만약 해당 파일이 압축파일일 경우 자동으로 풀리기도 함
- `ADD kaggle/ /kaggle/`
    - 로컬 호스트의 `kaggle/` 폴더를 컨테이너의 `/kaggle/` 폴더로 복사

✅ `RUN` : 이미지를 빌드할 때 실행할 명령어를 지정

- 주로 프로그램을 설치하거나 스크립트를 실행할 때 사용되고, 이 명령어를 쓸 때마다 새로운 레이어가 만들어짐!
- 🤔 ’새로운 레이어가 만들어진다’는 뜻은 도커의 이미지 관리 측면에서 이해해야 하는데, Docker 이미지는 여러 개의 읽기 전용 레이어로 이루어져 있어 각 레이어는 이전 레이어의 변경 사항을 기억한다. 예를 들어 하나의 이미지를 빌드할 때 **우분투 운영체제 → 패키지 설치 → 파일 복사** 단계를 거치게 되는데 이 한 스텝을 이 레이어(layer)가 관리한다는 의미다! 효율성, 캐싱을 고려했을 때 매우 합리적인 방법임이 분명한데 새로운 RUN 명령을 실행할 때마다 새로운 레이어가 생성되기 때문에, 불필요한 RUN 명령어를 많이 사용하면 이미지가 불필요하게 커질 수 있다. 따라서 **여러 명령을 한 RUN 명령어에 묶어서 실행**하는 것을 추천함!👍
- `RUN R -e "keras::install_keras(tensorflow = \"${TENSORFLOW_VERSION}\", extra_packages = c(\"pandas\"))"`
    - R을 실행해서 `keras`, `tensorflow`, `pandas` 같은 패키지를 설치함

✅ `ENV` : 컨테이너 안에서 사용할 환경 변수를 설정(docker run)

- 이 명령으로 설정된 환경 변수는 Docker 이미지 빌드 후 컨테이너 실행 시에도 유지가 가능함
- `ENV R_HOME=/usr/local/lib/R`
    - `R_HOME`이라는 환경 변수를 `/usr/local/lib/R`로 설정

✅ `ARG` : 이미지 빌드(docker build) 시에 사용할 변수 정의

- `ARG`로 정의된 변수는 빌드 타임(이미지를 만들 때)까지만 사용되고 실행될 때는 사용할 수 없음
- `ARG GIT_COMMIT=unknown
ARG BUILD_DATE_RSTATS=unknown`
    - `GIT_COMMIT`과 `BUILD_DATE_RSTATS`라는 이름을 가지고 있고, 기본값은 'unknown'

✅ `LABEL` : 이미지에 메타데이터(이미지를 설명하는 정보)를 추가함

- 이미지를 만든 날짜나 커밋 정보 등을 넣어서 버전 관리에 도움이 돼요. key-value 형식으로 데이터 저장
- `LABEL git-commit=$GIT_COMMIT
LABEL build-date=$BUILD_DATE_RSTATS`
    - `git-commit`과 `build-date`라는 이름으로 `GIT_COMMIT`과 `BUILD_DATE_RSTATS` 값을 라벨로 지정함

✅ `CMD` : 컨테이너가 시작될 때 기본적으로 실행할 명령을 지정

- `docker run` 명령어로 컨테이너를 실행할 때, 추가 명령을 지정하지 않으면 CMD에서 정한 명령어가 실행됨
- `CMD ["R"]`
    - 이 명령어는 컨테이너가 시작될 때 기본적으로 R 프로그램을 실행

---

이렇게 정리해봤는데, 지금 이 글을 읽고 있는 분이 있다면 이해가 되는지 걱정이다. 사실 명령어 개념 같은 부분은 실제로 내가 직접 실습을 해보는게 가장 빠른 이해 루트라고 생각하다. 그래서 나 역시 위에서 설치한 Kaggle RStudio의 도커파일을 분석해보기로 했다. 

아래 코드는 RStudio 서버 환경을 구축하기 위해 작성된 Dockerfile이다. 간단히 살펴보면 `FROM gcr.io/kaggle-gpu-images/rstats:${rstats_version}` 을 통해 베이스 이미지([gcr.io/kaggle-gpu-images/rstats](http://gcr.io/kaggle-gpu-images/rstats))를 설정하고 있다. 이는 Kaggle GPU 이미지를 기반으로 하여 R과 RStudio 서버를 사용할 수 있게 해준다. 바로 밑의 `RUN` 부분도 중요한데, 뒤의 명령어들을 통해 실질적으로 RStudio 서버를 다운로드 후 설치하고 있다. 관련 명령어들을 구체적으로 설명하지는 않을거지만(검색을 추천합니다!) 아까 위에서 레이어 개념을 설명했는데 연장선으로 `&&` 를 사용해 레이어를 줄이고 있다는 점도 확인 가능하다!

`WORKDIR /home/rstudio` 위에서 다룬 명령어는 아니지만 단어에서 유추할 수 있듯이 구체적으로 작업 디렉토리를 설정해 줄 수도 있다. 

`USER rstudio` 를 통해 해당 컨테이너의 사용자를 ‘rstudio’로 지정해 주었다. 기본적으로 Docker는 `root` 사용자로 실행된다. 다만 보안이나 파일 권한 문제로 인해 특정 작업을 일반 사용자로 지정하는 경우가 많은데 이 때 `USER` 명령어를 이용해 권한을 바꿔줄 수 있다. 

그 외에는 위에서 다뤘던 포트가 눈에 띄는데 `EXPOSE 8787` 지정해주면서 RStudio Server 쪽에 8787번 포트를 열어주었다.

👇 **RStudio server .Dockerfile**

```
  RG rstats_version=v56 

#base image(도커파일의 기본 베이스)는 from으로 불러오기
FROM gcr.io/kaggle-gpu-images/rstats:${rstats_version} 
ARG rstudio_version=2021.09.0-351
RUN apt-get update && \
    apt-get install -y gdebi-core && \
    wget https://download2.rstudio.org/server/focal/amd64/rstudio-server-${rstudio_version}-amd64.deb && \ 
    gdebi -n rstudio-server-${rstudio_version}-amd64.deb && \ 
    apt-get clean && \
    rm rstudio-server-${rstudio_version}-amd64.deb

WORKDIR /home/rstudio
COPY setup.R ./setup.R
RUN chown rstudio:rstudio ./setup.R
RUN chmod +x ./setup.R
USER rstudio
RUN Rscript ./setup.R && \
    rm ./setup.R
USER root


EXPOSE 8787
# LABEL revised_by="Daniel Youk" \
#       revised_date="2023-12-24"
ENTRYPOINT [ "/bin/bash"]
CMD ["-c", "rstudio-server start && tail -f /dev/null"]

```

그리고 한가지 중요한 점은 `CMD ["-c", "rstudio-server start && tail -f /dev/null"]` 이 명령어다. 위에서 `CMD` 를 ‘컨테이너가 시작될 때 기본적으로 실행할 명령을 지정’한다고 설명했다. 이 부분은 내 경험 상 예시 하나의 비교를 통해 바로 이해가 가능했다. 

아래 사진은 위에서 다룬 Kaggle R 이미지의 Dockerfile의 가장 마지막 줄의 캡쳐 부분이다. 보면 `CMD [”R”]` 이라고 적혀있는데, `CMD` 로 컨테이너 시작 명령어를 지정할 수 있기 때문에 해당 컨테이너를 실행하기 위한 기본 명령어(docker exec)를 응용해  `docker exec -it rstudio-container R` 이렇게 사용할 수 있다. 이 명령어로 바로  rstudio-container의 R 프로그램에 접근할 수 있는 것이다. 

<img src="{{ '/assets/p.14/p14.3.1.png' | relative_url }}" alt="Descriptive Alt Text" />

<img src="{{ '/assets/p.14/p14.6.png' | relative_url }}" alt="Descriptive Alt Text" />

그런데 RStudio Server를 구축하는 도커파일에도 `CMD` 명령어가 있다. 그런데 분명 생김새가 다른 걸로 보아 차이점이 있는 것이 확실하다.(뭐지..) 결론부터 말하자면 `CMD ["-c", "rstudio-server start && tail -f /dev/null"]` 명령어는 RStudio 서버를 시작하고 컨테이너가 종료되지 않게 유지하는 기능을 한다. `/dev/null`은 리눅스 시스템에서 특별한 파일로 입력된 데이터를 모두 버리는 역할을 하는데 `tail -f` 명령어로 `/dev/null` 파일을 계속 찾으려고 한다. 하지만 해당 파일은 앞서 말했듯이 무언가 입력되면 데이터를 모두 버리는 역할을 하기 때문에 무한루프에 빠지게 되고 컨테이너가 종료되지 않게 유지하는 기능을 할 수 있는 것이다!

<h4>3.1 Difference Between Docker CMD and ENTRYPOINT: When and How to Use Them?</h4>

Dockerfile을 작성할 때 CMD와 ENTRYPOINT 두 명령어가 계속 나왔다. **둘다 컨테이너가 시작될 때 실행될 명령을 지정**하는 데 사용되는 건 똑같지만 그 역할과 동작 방식에 차이가 있는 것 같다. 이 부분도 다시는 헷갈리기 싫으니 여기서 정리해 보려고 한다!

<h4>🚪 CMD</h4>

계속 언급되었던 CMD는 컨테이너가 실행될 때마다 수행되지만 덮어쓰기가 가능하다. 만약 사용자가 `docker run` 명령어를 실행하면서 다른 명령을 입력하면 CMD에서 지정한 명령은 실행되지 않고 새로 입력한 명령이 대신 실행이 가능하다. 예시를 통해 이해해보도록 하자!

우리는 `CMD ["R"]` 이 코드를 이용해 컨테이너가 시작되면 R 콘솔이 기본으로 실행됨을 위에서 확인했다. 그런데 만약 사용자가 컨테이너를 실행 하면서 다른 명령을 입력한다면 CMD에서 지정한 `R`은 무시되고 새로운 명령이 실행된다. 이게 무슨 말인가 하면 `docker run <image> /bin/bash` 로 명령어를 실행하면 `R` 대신 `/bin/bash`가 실행 된다는 말이다.

<h4>🚪 Entry Point</h4>

ENTRYPOINT는 항상 실행될 명령을 지정하는 데 사용된다. CMD와 달리 `docker run`에서 다른 명령을 입력해도 ENTRYPOINT에서 지정한 명령은 덮어쓰이지 않으며 그대로 실행되는 게 둘의 가장 큰 특징이다. 이것도 예시를 통해 이해해보록 하자!

`ENTRYPOINT ["/bin/bash", "-c"]` 이 코드는 컨테이너가 실행될 때 항상 <code>/bin/bash -c</code>를 실행하도록 한다. 그런데 만약 사용자가 `docker run`을 이용해 `docker run <image> "echo Hello"`를 실행하면 그 명령은 <code>/bin/bash -c</code> 의 인자로 전달되어 <code>/bin/bash -c "echo Hello"</code>로 실행된다!


👇 **summary**

<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CMD vs ENTRYPOINT</title>
    <style>
        table {
            width: 100%;
            border-collapse: collapse;
        }
        th, td {
            border: 1px solid #dddddd;
            text-align: left;
            padding: 8px;
        }
        th {
            background-color: #99CCFF;
        }
        td {
            vertical-align: top;
        }
    </style>
</head>
<body>

<table>
    <thead>
        <tr>
            <th>항목</th>
            <th>CMD</th>
            <th>ENTRYPOINT</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>목적</td>
            <td>기본 명령을 설정 (덮어쓰기 가능)</td>
            <td>필수 명령을 설정 (항상 실행됨)</td>
        </tr>
        <tr>
            <td>덮어쓰기</td>
            <td>`docker run` 명령어로 덮어씌움</td>
            <td>덮어쓰지 않고, 인자를 전달 가능</td>
        </tr>
        <tr>
            <td>유연성</td>
            <td>다른 명령으로 대체할 수 있음</td>
            <td>인자를 통해 명령 추가 가능</td>
        </tr>
        <tr>
            <td>예시</td>
            <td>`CMD ["R"]`</td>
            <td>`ENTRYPOINT ["/bin/bash", "-c"]`</td>
        </tr>
    </tbody>
</table>

</body>
</html>

결론적으로 두 명령어 모두 컨테이너가 시작될 때 실행될 명령을 지정하지만 CMD는 기본 실행 명령을, ENTRYPOINT는 항상 실행되어야 하는 필수 명령을 설정하는 데 사용된다는 점을 꼭기억하길 바란다! 아래 코드는 실제 프로젝트에서 많이 사용하는 조합이니 참고하면 좋을 듯 하다!

```
ENTRYPOINT ["/bin/bash", "-c"]
CMD ["rstudio-server start && tail -f /dev/null"]
```

---

<h2>4. Docker Pipeline</h2>
이번 포스팅을 마무리 하기 전에 지금까지의 학습 과정을 시각적으로 정리해 보려고 한다. 지금까지의 포스팅은 Dockerfile을 통해 이미지 생성하고 그 이미지를 통해 컨테이너 실행하는 부분까지 진행되었다. (Dockerfile → 이미지 생성 → Registry 저장 → 이미지 가져오기 → 컨테이너 실행) 

지금이야 이미지를 만들고 로컬에서 실행하는 데 큰 문제가 없었다. 하지만 컨테이너에서 사용하는 이미지들이 많아진다면 로컬이라는 한정된 공간에서 효율성이 떨어지게 된다. 그래서 보통은 Docker Registry라는 곳에 이미지를 저장(push)하고 필요할 때마다 당겨와(pull) 로컬 혹은 가상머신에서 사용하는 게 일반적이다. 다음 포스팅 부터는 이 이야기를 중심으로 진행되니 기대해 주길 바란다! 😎

<img src="{{ '/assets/p.14/p14.14.png' | relative_url }}" alt="Descriptive Alt Text" />

---

*이번 주차는 지금까지 도커를 공부하며 비유적으로 이해했던 부분들에 대해 조금 더 소프트웨어 측면에서 이론을 다루었다. 그러다보니 뭐랄까.. 분명 내가 알고 있던 내용이였던 것 같은데 또다시 새로운 내용이 되어버린 것 같았다. 특히 Dockerfile의 ENV, ARG 명령어에 대해서는 이미지를 빌드할 때, 컨테이너를 실행할 때로 구분되는데, 구체적으로 도커파일을 직접 생성해보면서 개념을 다질 필요가 있다고 느꼈다.* 

*아무튼 이번주차를 기점으로 도커 포스팅 시리즈도 반환점을 돌았다. 여전히 많이 부족한 글이지만 분명 글로 남기고 정리하는 건 이해의 농도가 다를 거라고 생각한다. 이번주는 유난히 뭘 해도 집중이 안되는 날이었는데 일단 버텨보는 걸로 하자!*👊

<img src="{{ '/assets/p.14/outro.gif' | relative_url }}" alt="Descriptive Alt Text" />

---
### Reference

[Kaggle_R_Dockerfile](References
https://github.com/Kaggle/docker-rstats/blob/main/Dockerfile)

[Please allow me to introduce myself: Torch for R](https://blogs.rstudio.com/ai/posts/2020-09-29-introducing-torch-for-r/)