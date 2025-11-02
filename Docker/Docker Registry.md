# 🐳 Docker Registry – Notes & Commands

---

## 🔹 1. What is Docker Registry?
- A **Docker Registry** is a storage & distribution system for Docker images.  
- It allows you to **push, pull, share, and manage images**.  
- **Docker Hub** is the default public registry.  
- You can also set up a **private registry** for your organization.

---


## 🔹 2. Components
- **Registry** → Stores images (e.g., Docker Hub, private registry).  
- **Repository** → Collection of images with the same name but different tags.  
- **Image** → A specific version of an application (e.g., `nginx:latest`).  

---

## 🔹 3. Why Use Docker Registry?
- Store and share images across teams.  
- Version control for images (tags).  
- Automate CI/CD workflows.  
- Keep private images for enterprise apps.  
- Save bandwidth by caching frequently used images locally.

---

## 🔹 4. Public Registries
- **Docker Hub** → `https://hub.docker.com/`  
- **GitHub Container Registry** → `ghcr.io`  
- **Google Container Registry (GCR)**  
- **Amazon Elastic Container Registry (ECR)**  
- **Azure Container Registry (ACR)**  

---

## 🔹 5. Private Registry Setup
Run a private registry container:
```bash
docker run -d -p 5000:5000 --name registry registry:2


6. Using Docker Registry
✅ Pull an Image from Docker Hub
docker pull nginx:latest

✅ Tag an Image for Registry
docker tag nginx:latest localhost:5000/mynginx:1.0

✅ Push Image to Registry
docker push localhost:5000/mynginx:1.0

✅ Pull Image from Registry
docker pull localhost:5000/mynginx:1.0

🔹 7. Docker Login & Logout
✅ Login to Registry
docker login


(For private registry, add docker login <registry_url>)

✅ Logout
docker logout

🔹 8. Managing Repositories & Tags
✅ List Local Images
docker images

✅ Remove Local Image
docker rmi <image_id>

✅ Retagging Images
docker tag alpine:latest myrepo/alpine:v1

🔹 9. Registry API (Optional)

Check images in registry:

curl http://localhost:5000/v2/_catalog


Check tags for repository:

curl http://localhost:5000/v2/mynginx/tags/list

🔹 10. Example: Push & Pull Flow

Pull base image:

docker pull alpine


Tag it:

docker tag alpine localhost:5000/myalpine:v1


Push to private registry:

docker push localhost:5000/myalpine:v1


Remove local image:

docker rmi localhost:5000/myalpine:v1


Pull back from registry:

docker pull localhost:5000/myalpine:v1

📌 Quick Summary

Registry = Storage for Docker images.

Docker Hub is the default public registry.

You can host your own private registry.

Common Commands:

docker login → Authenticate

docker pull → Download image

docker push → Upload image

docker tag → Rename/repository assignment
