# 🐳 Docker Compose – Notes & Commands

Docker Compose is a tool for **defining and running multi-container Docker applications** using a `docker-compose.yml` file.

---

## 🔹 1. Install & Verify
```bash
docker-compose --version

2. docker-compose.yml Structure

Example file:

version: "3.9"

services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: mydb
      MYSQL_USER: user
      MYSQL_PASSWORD: pass
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:

🔹 3. Common Docker Compose Commands
✅ Start Services
docker-compose up

✅ Start in Detached Mode
docker-compose up -d

✅ Stop Services
docker-compose down

✅ Stop & Remove Containers, Networks, Volumes
docker-compose down -v

✅ List Running Containers
docker-compose ps

✅ View Logs
docker-compose logs

✅ View Logs for a Specific Service
docker-compose logs web

✅ Scale Services
docker-compose up -d --scale web=3

✅ Restart Services
docker-compose restart

✅ Execute Command in a Service
docker-compose exec web bash

✅ Build Images
docker-compose build

✅ Rebuild Without Cache
docker-compose build --no-cache

✅ Pull Images
docker-compose pull

✅ Push Images
docker-compose push

✅ Check Config
docker-compose config

✅ Show Events (live container logs)
docker-compose events

🔹 4. Example: Simple Web + Redis
version: "3.8"

services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - redis

  redis:
    image: "redis:alpine"


Run:

docker-compose up -d

📌 Quick Summary

docker-compose up -d → Start in background.

docker-compose down → Stop & remove everything.

docker-compose ps → Check running services.

docker-compose logs → View logs.

docker-compose build → Build images.

docker-compose exec → Run inside a container.

Great for managing multi-container apps easily.
