Here's a comprehensive guide to Docker and its components, focusing on installation, image and container management, and deploying an Apache2 web application on Ubuntu.

---

## Docker Overview

### 1. What is Docker?
Docker is a platform designed to simplify application deployment by using **containers**. Containers package software with everything needed to run it, including code, runtime, libraries, and dependencies, ensuring consistent operation across different environments (development, testing, production).

### 2. Docker Platform Components
- **Docker Engine**: The core runtime environment for building and running containers.
- **Docker Hub**: A cloud-based registry for storing and sharing container images.
- **Orchestration Tools**: Tools like Docker Swarm and Kubernetes for managing large-scale container environments.

### 3. Docker Image
A Docker Image is a lightweight, standalone, executable package containing everything needed to run a piece of software. They are read-only, versioned, and can be built from a **Dockerfile**.

### 4. Docker Architecture
Docker uses a client-server architecture:
- **Docker Client**: The CLI for interacting with Docker.
- **Docker Daemon (dockerd)**: The background process managing containers and images.
- **Docker Registries**: Repositories for storing Docker images.

### 5. Docker Daemon
The Docker Daemon (`dockerd`) manages Docker containers, images, volumes, and networks, listening for requests from the Docker API (usually via the Docker client).

### 6. Docker Client
The Docker Client is the command-line interface (CLI) for issuing commands like `docker run`, `docker build`, and `docker push`.

### 7. Docker Desktop
Docker Desktop provides a user-friendly interface for managing Docker containers locally and is available for Windows and macOS.

### 8. Docker Registries
A Docker Registry is a centralized location for storing Docker images. The default public registry is Docker Hub, but private registries can also be set up.

### 9. Docker Objects
Key Docker objects include:
- **Images**: Templates for creating containers.
- **Containers**: Running instances of Docker images.
- **Networks**: Allow communication between containers.
- **Volumes**: Used for persistent data storage.

### 10. Docker Container
A Docker Container is a runnable instance of a Docker image, isolated and portable. They are lightweight and can be easily stopped or removed.

---

## Installing Docker Engine on Ubuntu

### Step 1: Set Up Docker's APT Repository

1. **Update the package index**:
   ```bash
   sudo apt-get update
   ```

2. **Install prerequisites**:
   ```bash
   sudo apt-get install ca-certificates curl
   ```

3. **Set up the keyring directory**:
   ```bash
   sudo install -m 0755 -d /etc/apt/keyrings
   ```

4. **Add Docker's official GPG key**:
   ```bash
   sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
   sudo chmod a+r /etc/apt/keyrings/docker.asc
   ```

5. **Add the Docker repository to APT sources**:
   ```bash
   echo \
   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
   $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

6. **Update the package index again**:
   ```bash
   sudo apt-get update
   ```

### Step 2: Install Docker Packages

To install the latest version of Docker and related tools:
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Step 3: Verify Docker Installation

Run the hello-world image to verify:
```bash
sudo docker run hello-world
```
If successful, you’ll see a confirmation message.

### Step 4: Managing Docker as a Non-Root User (Optional)

To run Docker commands without `sudo`:
1. Create the Docker group (if it doesn't exist):
   ```bash
   sudo groupadd docker
   ```

2. Add your user to the Docker group:
   ```bash
   sudo usermod -aG docker $USER
   ```

3. Log out and back in for changes to take effect.

---

## Managing Docker Images and Containers

### 1. Docker Images Commands

- **List Docker Images**:
  ```bash
  docker images
  ```

- **Pull an Image from Docker Hub**:
  ```bash
  docker pull httpd
  ```

- **Remove a Docker Image**:
  ```bash
  docker rmi <image_id>
  ```

- **Build Docker Image**:
  ```bash
  docker build -t my-apache-app:v1 .
  ```

### 2. Docker Container Commands

- **Run a Docker Container**:
  ```bash
  docker run -d -p 8080:80 --name my-apache-app httpd
  ```

- **View Running Containers**:
  ```bash
  docker ps
  ```

- **View All Containers (including stopped)**:
  ```bash
  docker ps -a
  ```

- **Stop a Running Container**:
  ```bash
  docker stop <container_name_or_id>
  ```

- **Remove a Container**:
  ```bash
  docker rm <container_name_or_id>
  ```

### 3. Docker Hub (Registry) Commands

- **Login to Docker Hub**:
  ```bash
  docker login
  ```

- **Push an Image to Docker Hub**:
  1. **Tag your image**:
     ```bash
     docker tag my-apache-app:v1 <your_dockerhub_username>/my-apache-app:v1
     ```

  2. **Push the image**:
     ```bash
     docker push <your_dockerhub_username>/my-apache-app:v1
     ```

### 4. Recap of Important Commands
- **List images**: `docker images`
- **Pull an image**: `docker pull <image_name>`
- **Run a container**: `docker run -d -p 8080:80 --name <container_name> <image_name>`
- **View running containers**: `docker ps`
- **Stop a container**: `docker stop <container_name_or_id>`
- **Remove a container**: `docker rm <container_name_or_id>`
- **Push an image to Docker Hub**: `docker push <your_dockerhub_username>/<repository_name>:<tag>`
- **Remove a Docker image**: `docker rmi <image_id>`

---

## Building and Running an Apache2 Web Application

### Directory Structure
Make sure you have the following structure:
```
my-apache-app/
├── Dockerfile
└── my_profile.tar.gz
```

### Dockerfile Example
Here’s the complete Dockerfile:
```Dockerfile
# Use Ubuntu as the base image
FROM ubuntu:latest

# Add metadata to the image using LABEL
LABEL "project"="Docker1"

# Update the package repository
RUN apt update

# Install Apache2
RUN apt install apache2 -y

# Start Apache2 in the foreground
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]

# Expose port 80
EXPOSE 80

# Set the working directory for Apache content
WORKDIR /var/www/html

# Create a volume for Apache log files
VOLUME /var/log/apache2

# Add a tar.gz archive containing your web content
ADD my_profile.tar.gz /var/www/html
```

### Building the Docker Image
Run the command to build the Docker image:
```bash
docker build -t my-apache-app:v1 .
```

### Verifying the Docker Image
Check available images:
```bash
docker images
```

### Running the Docker Container
Start the container with:
```bash
docker run -d -p 8080:80 --name apache-container my-apache-app:v1
```

### Accessing the Web Application
Visit `http://localhost:8080` in your browser to access the Apache web application.

### DockerHub (Registry) Commands
1. **Login to DockerHub**:
   ```bash
   docker login
   ```

2. **Tag the Image for DockerHub**:
   ```bash
   docker tag my-apache-app:v1 <your_dockerhub_username>/my-apache-app:v1
   ```

3. **Push the Image to DockerHub**:
   ```bash
   docker push <your_dockerhub_username>/my-apache-app:v1
   ```

### Docker Container Management
- **View Running Containers**:
  ```bash
  docker ps
  ```

- **Stop a Running Container**:
  ```bash
  docker stop apache-container
  ```

- **Remove a Stopped Container**:
  ```bash
  docker rm apache-container
  ```

---
