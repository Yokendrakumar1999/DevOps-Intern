Docker and its components:

1. What is Docker?

Docker is a platform designed to simplify application deployment by using containers. Containers package software with everything needed to run it, including code, runtime, libraries, and dependencies. This ensures that the software works consistently across different computing environments (development, testing, production, etc.).

2. Docker Platform

The Docker platform consists of tools and components that help developers and IT operations teams to build, ship, and run applications efficiently. The platform includes:

Docker Engine: The core runtime environment that enables building and running containers.

Docker Hub: The default cloud-based registry for storing and sharing container images.

Orchestration Tools: Such as Docker Swarm and Kubernetes, used for managing large-scale containerized environments.

3. Docker Image

A Docker Image is a lightweight, standalone, executable package that includes everything needed to run a piece of software: the code, runtime, libraries, environment variables, and configuration files. It’s essentially a blueprint used to create containers.

Images are read-only and versioned. They can be built from a Dockerfile and stored in a registry like Docker Hub.

4. Docker Architecture

Docker follows a client-server architecture:

Docker Client: The CLI that interacts with the Docker Daemon.

Docker Daemon (dockerd): The background process that handles container management.

Docker Registries: Repositories where Docker images are stored.

The Docker client communicates with the Docker daemon, which manages Docker objects such as containers, images, networks, and storage.





5. Docker Daemon

The Docker Daemon (dockerd) is the background process responsible for managing Docker containers, images, volumes, and networks. It listens for requests from the Docker API (often via the Docker client) and performs the necessary actions to build, run, and manage containers.

6. Docker Client

The Docker Client is a command-line interface (CLI) that users interact with to issue commands like docker run, docker build, and docker push. The client sends these commands to the Docker Daemon, which executes them.

7. Docker Desktop

Docker Desktop is a complete, easy-to-use development environment for building and managing Docker containers locally. It includes Docker Engine, Docker CLI, and a graphical interface to help developers easily manage containers and images.

Available for Windows and macOS.

Simplifies container management on local machines by integrating tools and runtimes in a user-friendly interface.

8. Docker Registries

A Docker Registry is a centralized location where Docker images are stored. These images can be pushed to a registry or pulled from it for use.

Public Registry: Docker Hub is the default and most commonly used public registry.

Private Registry: You can also host your own private registry to store images securely.

9. Docker Objects

Docker objects are essential components in the Docker ecosystem, including:

Images: Templates used to create containers.

Containers: Running instances of Docker images.

Networks: Allow containers to communicate with each other.

Volumes: Used for persistent data storage.

10. Docker Container

A Docker Container is a runnable instance of a Docker image. Containers are isolated and portable, meaning they run the same way regardless of where they are deployed. They include the application and its dependencies, and run on top of the host operating system’s kernel.

Key Points:

Ephemeral: Containers are lightweight and can be stopped or removed easily.

Isolated: Containers run in isolation from each other and the host system, but can communicate through networks.

Portable: Containers can be transferred and run across different machines, ensuring consistent behavior.

Guide to installing Docker Engine on an Ubuntu host machine. This includes setting up the Docker repository, installing Docker, and verifying the installation.

Installing Docker Engine on Ubuntu

Step 1: Set Up Docker's APT Repository

Update the package index:
Open a terminal and run the following command to update the package list:

sudo apt-get update

Install prerequisites:
Install the necessary packages to allow apt to use a repository over HTTPS:

sudo apt-get install ca-certificates curl

Set up the keyring directory:
Create a directory for the Docker GPG key:

sudo install -m 0755 -d /etc/apt/keyrings

Add Docker's official GPG key:
Download and add the Docker GPG key:

sudo curl -fsSL <https://download.docker.com/linux/ubuntu/gpg> -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

Add the Docker repository to Apt sources:
Execute the following command to add the Docker repository:

echo \\
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] <https://download.docker.com/linux/ubuntu> \\
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \\
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

Step 2: Install Docker Packages

To install the latest version of Docker and related tools, run the following command:

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

Step 3: Verify Docker Installation

To ensure that Docker has been installed correctly, run the hello-world image:

sudo docker run hello-world

This command downloads a test image from Docker Hub and runs it in a container. If the installation is successful, you will see a confirmation message indicating that Docker is working properly.

Step 4: Managing Docker as a Non-Root User (Optional)

If you want to run Docker commands without needing to prepend sudo, you can add your user to the docker group:

Create the docker group (if it doesn’t exist):

sudo groupadd docker

Add your user to the docker group:
Replace your-username with your actual username:

sudo usermod -aG docker $USER

Log out and log back in or restart your terminal for the changes to take effect.

Managing Docker images, containers, Docker Hub (registry) operations, and building a Dockerized web application using Apache2 

1. Docker Images Commands

Docker images are templates used to create containers.

List Docker Images:

docker images

This command lists all images available on your system.

Pull an Image from Docker Hub:

docker pull <image_name>

Example:

docker pull httpd

This pulls the Apache HTTP Server image from Docker Hub.

Remove a Docker Image:

docker rmi <image_id>

Example:

docker rmi a9b44a889f8d

This deletes the specified image from your system.

Build Docker Image

Run the docker build command to build the Docker image based on this Dockerfile.

docker build -t my-apache-app:v1 .

-t my-apache-app:v1: Tags the image as my-apache-app with the version v1.

.: Refers to the current directory where the Dockerfile is located.

2. Docker Container Commands

A Docker container is a running instance of a Docker image.

Run a Docker Container:

docker run -d -p <host_port>:<container_port> --name <container_name> <image_name>

Example:

docker run -d -p 8080:80 --name my-apache-app httpd

This runs the Apache server in a container, accessible at <http://localhost:8080.>

View Running Containers:

docker ps

View All Containers (including stopped):

docker ps -a

Stop a Running Container:

docker stop <container_name_or_id>

Remove a Container:

docker rm <container_name_or_id>

3. Docker Hub (Registry) Commands

Docker Hub is where images can be stored, shared, and accessed.

Login to Docker Hub:

docker login

You will be prompted to enter your Docker Hub username and password.

Push an Image to Docker Hub:

First, tag your image:

docker tag <image_name> <dockerhub_username>/<repository_name>:<tag>

Example:

docker tag my-apache-app abc123/apache-app:latest

Push the image to Docker Hub:

docker push <dockerhub_username>/<repository_name>:<tag>

Example:

docker push abc123/apache-app:latest

Pull an Image from Docker Hub:

docker pull <dockerhub_username>/<repository_name>:<tag>

Example:

docker pull abc123/apache-app:latest

4. Recap of Important Commands

List images: docker images

Pull an image: docker pull <image_name>

Run a container: docker run -d -p <host_port>:<container_port> --name <container_name> <image_name>

View running containers: docker ps

Stop a container: docker stop <container_name_or_id>

Remove a container: docker rm <container_name_or_id>

Push an image to Docker Hub: docker push <dockerhub_username>/<repository_name>:<tag>

Remove a Docker image: docker rmi <image_id>

To build a Docker image for an Apache2 web application on Ubuntu using the provided Dockerfile, here’s the detailed breakdown, including commands for managing Docker images, containers, and Docker Hub (Docker registry).

 1. Prepare the Directory Structure

Make sure you have the following structure in your project directory:

my-apache-app/ 
├── Dockerfile 
└── my_profile.tar.gz

Dockerfile: This is the file where you define your image.

my_profile.tar.gz: This archive should contain your web application files (e.g., HTML, CSS, JS, etc.).

2. Dockerfile Example

Here’s the complete Dockerfile based on your instructions:

# Use Ubuntu as the base image
FROM ubuntu:latest

# Add metadata to the image using LABEL
LABEL "project"="Docker1"

# Update the package repository
RUN apt update

# Install Apache2
RUN apt install apache2 -y

# Start Apache2 in the foreground to keep the container running
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]

# Expose port 80 to allow access to the web server
EXPOSE 80

# Set the working directory for Apache content
WORKDIR /var/www/html

# Create a volume for Apache log files
VOLUME /var/log/apache2

# Add a tar.gz archive containing your web content
ADD my_profile.tar.gz /var/www/html

3. Build Docker Image

Run the docker build command to build the Docker image based on this Dockerfile.

docker build -t my-apache-app:v1 .

-t my-apache-app:v1: Tags the image as my-apache-app with the version v1.

.: Refers to the current directory where the Dockerfile is located.

4. Verify the Docker Image

Once the image is built, verify it by running the following command:

docker images

This will list all available images on your system, including the newly created my-apache-app:v1.

5. Run Docker Container

Now that the image is ready, you can run it as a container. This will start Apache2 and serve any content extracted from my_profile.tar.gz.

docker run -d -p 8080:80 --name apache-container my-apache-app:v1

-d: Runs the container in detached mode.

-p 8080:80: Maps port 8080 on the host machine to port 80 in the container, making the web server accessible via <http://localhost:8080.>

--name apache-container: Gives the container a name for easier management.

my-apache-app:v1: The image built earlier.

6. Access the Web Application

Now, you can access the Apache web application by visiting <http://localhost:8080> in your browser.

6. DockerHub (Registry) Commands

Login to DockerHub

Before you push your image to DockerHub, make sure you’re logged in.

docker login

Tag the Image for DockerHub

Tag the image so you can push it to your DockerHub repository:

docker tag my-apache-app:v1 <your_dockerhub_username>/my-apache-app:v1

Push the Image to DockerHub

Push the tagged image to DockerHub:

docker push <your_dockerhub_username>/my-apache-app:v1

This uploads your image to your DockerHub repository.

7. Docker Container Management

View Running Containers

To see all currently running containers:

docker ps

Stop a Running Container

To stop a container, use the following command:

docker stop apache-container

Remove a Stopped Container

To remove a container once it has been stopped:

docker rm apache-container



Build Image:

docker build -t my-apache-app:v1 .

Run Container:

docker run -d -p 8080:80 --name apache-container my-apache-app:v1

View Docker Images:

docker images

Push to DockerHub:

docker push <your_dockerhub_username>/my-apache-app:v1

Stop and Remove Container:

docker stop apache-container
docker rm apache-container