---
layout: post
title:  "Introduction to Docker"
categories: [Blog]
tags: [docker]
---

# Introduction to Docker


In this guide, we’ll introduce Docker, a tool that’s revolutionizing how people create and manage software. By the end of this guide, you’ll understand the basics of Docker and containers, giving you a solid foundation to use this essential tool in today’s development world.

## What's Docker?

Docker is a tool that simplifies the process of building, launching, and running applications by using *containers*. Think of a container as a box that holds everything an application needs to run, such as files, environment variables, settings, and dependencies, ensuring it works the same on any computer.

This approach solves the common problem of “But it works on my machine!” by ensuring the app behaves consistently, no matter where it runs. Containers offer many benefits: they maintain consistency through different stages of development and make better use of a computer’s resources. For instance, if you mess everything up, instead of potentially damaging your main operating system, you can just delete the container and recreate it. Problem solved!

### How is Docker structured?

- **Docker Images**: An image is like a blueprint for your application. It contains everything needed to run the app, including code, libraries, and configuration files. Once created, images don’t change, ensuring consistency.

- **Containers**: A container is an active version of an image. Think of it as the image in action—running the app and keeping it separate from everything else on the computer, ensuring it works consistently, even if the environment changes.

- **Dockerfiles**: A Dockerfile is a script with a list of instructions that Docker uses to create the image. By using a Dockerfile, you can automate image creation, making sure each image is built the same way every time. Images can be shared on the *Docker Hub*.

### Docker or Virtual Machines?
Both containers and virtual machines (VMs) allow applications to run independently, but they do it differently. VMs include a full operating system, so they can be slow to start and use a lot of resources. Containers, however, share the host computer’s operating system and only hold the app and its essentials, making them much lighter and quicker to launch. 

## Let's install Docker!
---
### Install Docker on Linux

1. **Update the Package Index**  
    Start by updating your package list to ensure you’re getting the latest versions:
    ```bash
    sudo apt update
    ```

2. **Add Docker's GPG Key**  
    Install the necessary packages to allow Docker’s repositories to be managed over HTTPS:
    ```bash
     sudo apt-get install ca-certificates curl
     sudo install -m 0755 -d /etc/apt/keyrings
     sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
     sudo chmod a+r /etc/apt/keyrings/docker.asc
    ```

3. **Add the Repository to APT Sources**  
    ```bash
     echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
     $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
     sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
     sudo apt-get update
    ```

4. **Install Package**  
    Update the package index again, and then install Docker:
    ```bash
     sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```

5. **Done!**
     If the installation was successful, you should now be able to run a test image called "Hello World". Let's try it!
    ```bash
     sudo docker run hello-world
    ```

---

### Install Docker on Windows

1. **Download the Docker Installer**  
     Visit the [Docker Official Webpage](https://docs.docker.com/desktop/install/windows-install/) and click on "Download for Windows".

2. **Run the Installer**  
     Execute the downloaded installer and follow the on-screen instructions. Wait for the installation process to complete.

3. **Verify Installation**  
     Once the installation is finished, you should see Docker in the search bar. Open Docker to ensure it is installed correctly.

## Docker Commands
---
Now that you've installed Docker, here are some essential commands to get you started:

| Command                          | Description                                               |
|----------------------------------|-----------------------------------------------------------|
| `docker --version`               | Show the Docker version installed                         |
| `docker pull <image>`            | Download an image from Docker Hub                         |
| `docker run <image>`             | Run a container from an image                             |
| `docker ps`                      | List running containers                                   |
| `docker ps -a`                   | List all containers (running and stopped)                 |
| `docker stop <container_id>`     | Stop a running container                                  |
| `docker rm <container_id>`       | Remove a stopped container                                |
| `docker rmi <image_id>`          | Remove an image                                           |
| `docker build -t <tag> .`        | Build an image from a Dockerfile in the current directory |
| `docker exec -it <container_id> <command>` | Run a command in a running container                |


## Write a Dockerfile
---

A Dockerfile is a script that contains a series of instructions on how to build a Docker image.
### Step 1: Choose a Base Image

The first instruction in a Dockerfile is usually the `FROM` command, which specifies the base image to use. For example, to use the latest version of Ubuntu as the base image, you would write:

```Dockerfile
FROM ubuntu:latest
```

### Step 2: Install Dependencies

Next, you can use the `RUN` command to install any dependencies your application needs. For example, to update the package list and install Apache2, you would write:

```Dockerfile
RUN apt-get update && apt-get install -y apache2
```

### Step 3: Add Your Application Code

You can use the `COPY` or `ADD` command to add your application code to the image. For example, to copy a file named `index.html` from your local machine to the `/var/www/html` directory in the image, you would write:

```Dockerfile
COPY index.html /var/www/html/
```

### Step 4: Expose Ports

If your application listens on a specific port, you can use the `EXPOSE` command to make that port available to the outside world. For example, to expose port 80, you would write:

```Dockerfile
EXPOSE 80
```

### Step 5: Specify the Command to Run

Finally, you can use the `CMD` or `ENTRYPOINT` command to specify the command that should be run when the container starts. For example, to start Apache2 in the foreground, you would write:

```Dockerfile
CMD ["apache2ctl", "-D", "FOREGROUND"]
```

### Example Dockerfile

Here’s an example Dockerfile that puts all these steps together:

```Dockerfile
# Use the official Ubuntu base image
FROM ubuntu:latest

# Update the package list and install Apache2
RUN apt-get update && apt-get install -y apache2

# Copy the application code
COPY index.html /var/www/html/

# Expose port 80
EXPOSE 80

# Start Apache2 in the foreground
CMD ["apache2ctl", "-D", "FOREGROUND"]
```

### Table Listing Common Terms in Dockerfile

| Term         | Description                                                                 |
|--------------|-----------------------------------------------------------------------------|
| `FROM`       | Specifies the base image to use for the Docker image.                       |
| `RUN`        | Executes commands in a new layer on top of the current image and commits the results. |
| `COPY`       | Copies new files or directories from the source path and adds them to the filesystem of the container. |
| `ADD`        | Similar to `COPY`, but also supports extracting tar files and downloading remote files. |
| `CMD`        | Provides the command that will be executed when a container is run.         |
| `ENTRYPOINT` | Configures a container that will run as an executable.                      |
| `EXPOSE`     | Informs Docker that the container listens on the specified network ports at runtime. |
| `ENV`        | Sets environment variables.                                                 |
| `WORKDIR`    | Sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, and `ADD` instructions that follow. |
| `VOLUME`     | Creates a mount point with the specified path and marks it as holding externally mounted volumes from native host or other containers. |
| `USER`       | Sets the user name or UID to use when running the image and for any `RUN`, `CMD`, and `ENTRYPOINT` instructions that follow. |
| `ARG`        | Defines a variable that users can pass at build-time to the builder with the `docker build` command. |
| `ONBUILD`    | Adds a trigger instruction to the image that will be executed when the image is used as the base for another build. |





## Let's make our first container
---

### Running an Apache2 Container

In this section, we'll guide you through creating and running an Apache2 container using Docker.

#### Step 1: Create a Dockerfile

Create a new directory for your project and navigate into it:

```bash
mkdir apache2-container
cd apache2-container
```

Create a file named `Dockerfile` in this directory:

```bash
touch Dockerfile
```

#### Step 2: Write the Dockerfile

Open the `Dockerfile` in your favorite text editor and add the following content:

```Dockerfile
# Use the official Ubuntu base image
FROM ubuntu:latest

# Update the package list and install Apache2
RUN apt-get update && apt-get install -y apache2

# Expose port 80 to the outside world
EXPOSE 80

# Start Apache2 in the foreground
CMD ["apache2ctl", "-D", "FOREGROUND"]
```

### Explanation of the Dockerfile

- `FROM ubuntu:latest`: This line specifies the base image to use for the container. We're using the latest version of Ubuntu.
- `RUN apt-get update && apt-get install -y apache2`: This line updates the package list and installs Apache2.
- `EXPOSE 80`: This line tells Docker to expose port 80, which is the default port for HTTP traffic.
- `CMD ["apache2ctl", "-D", "FOREGROUND"]`: This line starts Apache2 in the foreground, ensuring that the container keeps running.

### Step 3: Build the Docker Image

Build the Docker image using the Dockerfile:

```bash
docker build -t my-apache2 .
```

### Step 4: Run the Docker Container

Run a container from the image you just built:

```bash
docker run -d -p 8080:80 my-apache2
```

- **8080**: This is the port on the host machine that will be exposed and accessible. It means that the host machine will have port 8080 open.
- **80**: This is the port inside the container that will be open. It is not directly exposed on the host machine but mapped to port 8080.

### Step 5: Verify the Container is Running

First of all, you should run the following command to check if the container is running:
```bash
docker ps
```
If it says that it's running, you can open your web browser and navigate to `http://localhost:8080` (or the IP Address of your machine). You should see the default Apache2 welcome page.

Congratulations! You've successfully created and run an Apache2 container using Docker.

---