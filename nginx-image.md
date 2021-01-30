## Assignment - Using containers, building your own image to host your pages, and store it in a Git repo

Created for Web Application Design 354 at Illinois State University 

#### What You Need to Know

Now that you have learned the basics of pulling and using container images, this lab will ask you to practice your new knowledge of Docker containers. After refreshing your skills, we will build a container image containing your HTML source code created in Assignment 2. That container will launch a Web server and serve your pages. Finally, we will take the Dockerfile and other source code used to create your Docker image and upload them to a repository that you create. 

![grab screen snip here](./images/sm-camera.png)

Since this assignment is part tutorial and part tasks for you to complete, you will need to take snapshots along the way to prove you worked through the steps. Whenever you see this camera symbol, please take a snapshot or screen snippet and place it in a folder you will later submit.

### Getting Started
NGINX (pronounced "engine-X") is one of the most popular web servers in the world. In this tutorial we will take a look at the NGINX Official Docker Image and how to use it. We’ll start by running a static web server locally then we’ll build a custom image to house our web server and the files it needs to serve.

#### Prerequisites
To complete this tutorial, you will need the following:

Free Docker account
Docker running locally
Unix/Linux Terminal
VS Code
Git installed and operating from your terminal

Run the following command to test that Docker is installed and working well on your system. 
```
docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete
Digest: sha256:31b9c7d48790f0d8c50ab433d9c3b7e17666d6993084c002c2ff1ca09b96391d
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 ...
```
## 1.0 Finding a base image

The Docker Official Images are a curated set of Docker repositories hosted on Docker Hub that have been scanned for vulnerabilities and are maintained by Docker employees and upstream maintainers.

Official Images are a great place for new Docker users to start. These images have clear documentation, promote best practices, and are designed for the most common use cases.

Let’s take a look at the NGINX official image. Open your favorite browser and log into Docker. If you do not have a Docker account yet, you can create one for free.

Once you have logged into Docker, enter “NGINX” into the top search bar and press enter. The official NGINX image should be the first image in the search results. You will see the “OFFICIAL IMAGE” label in the top right corner of the search entry.

![Official Docker NGINX base image](https://lh6.googleusercontent.com/FJh7W5WYZ06vCV241juS-hTPbyGgzkCJ3671kqEmW7-Vo2dOzRaTvUsFymqZiNtbktHq7JYU-1KsRd9fWxuIX5Aicvlz1UwzOp0ryR38NTwrnvhC4pAsnkg1UfJ4xWccgFgPj5Gj)

###### Running a basic web server
Let’s run a basic web server using the official NGINX image. 

First, let's test our image name by trying to do a pull. This will download the container image and save it on your drive. This will test that we have the right image name as it exists in the Docker repositories. 

```
$ docker pull nginx
```
Next check which Docker images are downloaded and saved to your hard drive by doing the following command. 
```
docker images
REPOSITORY   TAG          IMAGE ID       CREATED       SIZE
python       3            4b9378be0bb9   4 days ago    885MB
php          7.4-apache   899ab23566b7   8 days ago    414MB
nginx        latest       f6d0b4767a6c   2 weeks ago   133MB
```
Since that worked, let's test running this.  Run the following command to start the container and take a screen snippet or capture that shows a successful pull and images on your system.

![grab screen snip here](./images/sm-camera.png)

#### 1.1 Docker Run
Great! Let's now run a Docker **container** based on this image. To do that, you are going to use the `docker run` command.
```
$ docker run -it --rm -d -p 8080:80 --name web nginx
```
With the above command, you started running the container as a daemon (-d) and published port 8080 on the host network. You also named the container 'web' using the --name option. 

Open your favorite browser and navigate to http://localhost:8080 .   You should see the following NGINX welcome page.

![Default nginx running](https://lh4.googleusercontent.com/TBXwvnvDDxQAMJdPnPHkM8iG-z9_qKtjb7dfyu36eV1K0dutD5YE20v_9WRZUEvWHepXq86rbPKtMiBkZFVOsEk68iHA3X7uxmQ_Ponm87kqJhtexPALT-_A_K2fOCvoD3L9ks4g)

![grab screen snip here](./images/sm-camera.png)

What happened? Behind the scenes, a lot of stuff happened. When you call `run`,
1. The Docker client contacts the Docker daemon
2. The Docker daemon checks local store to see if the image (nginx in this case) is available locally, and if not, downloads it from Docker Store. (Since we have issued `docker pull nginx` before, the download step is not necessary)
3. The Docker daemon creates the container and then runs a command in that container.
4. The Docker daemon streams the output of the command to the Docker client

Run the following command in order to list the Docker containers on your system. 
```
docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                  NAMES
03fc46c7889d   nginx     "/docker-entrypoint.…"   6 minutes ago   Up 6 minutes   0.0.0.0:8080->80/tcp   web
```
This is great, but the purpose of running a web server is to serve our own custom html files and not the default NGINX welcome page.

Let’s stop the container and take a look at serving our own HTML files.
```
$ docker stop web
```
Ok, now it's time to see the `docker ps` command. The `docker ps` command shows you all containers that are currently running.

```
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

Since no containers are running, you see a blank line.

You should also be aware that running the `run` command with the `-it` flags attaches to an interactive tty in the container. This is often used to run the shell and run many commands in the container. 

That concludes a whirlwind tour of the `docker run` command which would most likely be the command you'll use most often. It makes sense to spend some time getting comfortable with it. To find out more about `run`, use `docker run --help` to see a list of all flags it supports. As you proceed further, we'll see a few more variants of `docker run`.

### 1.2 Terminology
In the last section, you saw a lot of Docker-specific jargon which might be confusing to some. So before you go further, let's clarify some terminology that is used frequently in the Docker ecosystem.

- *Images* - The file system and configuration of our application which are used to create containers. To find out more about a Docker image, run `docker inspect nginx`. In the demo above, you used the `docker pull` command to download the **nginx** image. When you executed the command `docker run hello-world`, it also did a `docker pull` behind the scenes to download the **hello-world** image.
- *Containers* - Running instances of Docker images &mdash; containers run the actual applications. A container includes an application and all of its dependencies. It shares the kernel with other containers, and runs as an isolated process in user space on the host OS. You created a container using `docker run` which you did using the nginx image that you downloaded. A list of running containers can be seen using the `docker ps` command.
- *Docker daemon* - The background service running on the host that manages building, running and distributing Docker containers.
- *Docker client* - The command line tool that allows the user to interact with the Docker daemon.
- *Docker Store* - A [registry](https://store.docker.com/) of Docker images, where you can find trusted and enterprise ready containers, plugins, and Docker editions. You'll be using this later in this tutorial.

## Next Step
For the next step in the tutorial, head over to [2.0 Build Your Image](./build-image.md)
