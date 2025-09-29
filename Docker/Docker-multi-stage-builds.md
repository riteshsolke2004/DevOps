# 🐳 Multi-Stage Docker Builds – Notes & Commands

---

## 🔹 1. What are Multi-Stage Builds?
- A **multi-stage build** allows you to use **multiple FROM statements** in a single `Dockerfile`.  
- Each `FROM` creates a new build stage.  
- You can **copy only the required artifacts** (binaries, compiled code) into the final image.  
- This reduces **image size** and makes production images cleaner.

---

## 🔹 2. Why Use Multi-Stage Builds?
- Smaller final images → faster deployment.  
- Keeps only runtime dependencies in the final image.  
- Avoids leaking unnecessary tools like compilers.  
- Simplifies Dockerfile (no need for separate build scripts).  
- Best for compiled languages like Go, Java, C++, but also useful for Node.js & Python.

---

## 🔹 3. Basic Example (Go App)

### Dockerfile
```dockerfile
# Stage 1: Build
FROM golang:1.20 AS builder
WORKDIR /app
COPY . .
RUN go build -o myapp

# Stage 2: Run
FROM alpine:latest
WORKDIR /app
COPY --from=builder /app/myapp .
CMD ["./myapp"]

Build & Run
docker build -t myapp:multi .
docker run --rm myapp:multi

🔹 4. Example with Node.js (Production Build)
Dockerfile
# Stage 1: Build React app
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Serve using nginx
FROM nginx:alpine
COPY --from=builder /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

Build & Run
docker build -t react-app:multi .
docker run -d -p 8080:80 react-app:multi

🔹 5. Named Build Stages

You can name stages for clarity.

FROM golang:1.20 AS build-stage
WORKDIR /src
COPY . .
RUN go build -o server

FROM alpine AS runtime
WORKDIR /app
COPY --from=build-stage /src/server .
CMD ["./server"]

🔹 6. Copying Specific Files
COPY --from=build-stage /src/config.yml /app/config.yml

🔹 7. Build Commands
✅ Build Image Normally
docker build -t myimage .

✅ Target a Specific Stage
docker build --target build-stage -t intermediate-image .

✅ Reduce Layers with Multi-Stage
docker build -t optimized-app .

🔹 8. Multi-Stage with Python Example
# Stage 1: Build dependencies
FROM python:3.10-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --prefix=/install -r requirements.txt

# Stage 2: Final image
FROM python:3.10-slim
WORKDIR /app
COPY --from=builder /install /usr/local
COPY . .
CMD ["python", "app.py"]

📌 Quick Summary

Multi-stage builds use multiple FROM statements.

First stage = build environment, last stage = production environment.

Commands:

docker build -t app:multi . → Build final image

docker build --target <stage> -t intermediate . → Build only up to a specific stage

Keeps images small, clean, and secure.
