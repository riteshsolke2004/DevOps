# 📦 Introduction to Docker

## 🔹 What is Docker?
- Docker is a **platform for developing, shipping, and running applications**.
- Uses **containerization** to package applications with all dependencies.
- Solves the "works on my machine" problem.

---

## 🔹 Why Docker?
- Lightweight alternative to virtual machines.
- Provides **isolation** between applications.
- Ensures **consistency** across environments (dev, test, prod).
- Faster deployment and scaling.

---

## 🔹 Key Concepts

### 1. Container
- Lightweight, portable unit that runs an application.
- Shares the host OS kernel but isolated from other containers.

### 2. Image
- Blueprint for containers.
- Contains code, runtime, libraries, and dependencies.
- Immutable once built.

### 3. Dockerfile
- Script to build images automatically.
- Contains instructions like:
  ```dockerfile
  FROM ubuntu:20.04
  RUN apt-get update && apt-get install -y python3
  COPY . /app
  CMD ["python3", "app.py"]



4. Docker Engine

Core part of Docker: client + server (daemon).

5. Registry

Repository for images (e.g., Docker Hub).

🔹 Docker Architecture

Client: Runs commands (docker run, docker build).

Daemon (dockerd): Manages containers and images.

Registry: Stores Docker images.

Objects: Images, Containers, Volumes, Networks.



#Basic Commands 

# Pull an image
docker pull ubuntu

# Run a container
docker run -it ubuntu bash

# List running containers
docker ps

# List all containers
docker ps -a

# Build an image
docker build -t myapp .

# Stop a container
docker stop <container_id>

# Remove a container
docker rm <container_id>

# Remove an image
docker rmi <image_id>


🔹 Volumes & Persistence

Containers are ephemeral (data lost when stopped).

Volumes store data outside the container lifecycle.

docker run -v /host/path:/container/path myapp


# 🐳 Docker Image vs Docker Container

## 🔹 Docker Image
- A **read-only template** that contains:
  - Application code
  - Dependencies
  - Runtime environment
  - Configuration files
- Blueprint used to create containers.
- Immutable (cannot be changed once built).
- Can be versioned, shared, and reused (e.g., Docker Hub).

👉 Think of an image as a **class** in programming.

Example:
```bash
docker pull nginx
docker images
🔹 Docker Container
A running instance of an image.

Lightweight and isolated environment.

Has its own filesystem, networking, and processes.

Ephemeral by default (data lost when stopped unless volumes used).

You can create, start, stop, move, or delete containers.

👉 Think of a container as an object created from a class.

Example:

bash
Copy code
docker run -d --name mynginx nginx
docker ps
🔹 Key Difference
Aspect	Docker Image	Docker Container
Definition	Blueprint/template for containers	Running instance of an image
State	Static (read-only)	Dynamic (can start/stop/remove)
Mutability	Immutable	Writable (changes stored as new layer)
Storage	Stored in registry (e.g., Docker Hub)	Runs on host system

✅ Summary
Image = Template

Container = Running Instance

One image can create multiple containers.
