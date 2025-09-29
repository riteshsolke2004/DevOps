# 🐳 Docker Storage & Volumes – Complete Notes

Docker provides multiple mechanisms to handle **data persistence and storage**.

---

## 🔹 1. Storage Options in Docker
Docker stores data in three main ways:

1. **Volumes** → Managed by Docker (`/var/lib/docker/volumes/`).
2. **Bind Mounts** → Mounts host paths directly into a container.
3. **tmpfs Mounts** → Stores data in host RAM (temporary).

---

## 🔹 2. Docker Volumes
Volumes are the **preferred method** for persistent storage.

### ✅ Create a Volume
```bash
docker volume create my_volume

✅ List Volumes
docker volume ls

✅ Inspect a Volume
docker volume inspect my_volume

✅ Remove a Volume
docker volume rm my_volume

✅ Remove Unused Volumes
docker volume prune

✅ Run Container with a Volume
docker run -d --name my_container -v my_volume:/app/data nginx

🔹 3. Bind Mounts

Bind mounts map host directories/files into containers.

✅ Run Container with Bind Mount
docker run -d --name my_container \
  -v /path/on/host:/path/in/container \
  nginx

✅ Read-Only Bind Mount
docker run -d --name my_container \
  -v /path/on/host:/path/in/container:ro \
  nginx

🔹 4. tmpfs Mounts

tmpfs mounts keep data only in memory (RAM).

✅ Run Container with tmpfs
docker run -d --name my_container \
  --tmpfs /app/cache \
  nginx

🔹 5. Volumes in Docker Compose

Example docker-compose.yml:

version: "3.8"

services:
  web:
    image: nginx
    volumes:
      - my_volume:/usr/share/nginx/html

volumes:
  my_volume:

🔹 6. Copy Data Between Host & Container
✅ Copy from Host → Container
docker cp ./localfile.txt my_container:/app/localfile.txt

✅ Copy from Container → Host
docker cp my_container:/app/localfile.txt ./localfile.txt

🔹 7. Inspect Container Mounts
docker inspect my_container


Check the "Mounts" section for details.

🔹 8. Docker Storage Drivers

Storage drivers manage how images and layers are stored.

overlay2 (default, recommended for modern Linux).

aufs (older, deprecated).

btrfs / zfs (advanced filesystems with snapshots).

devicemapper (used in older Docker setups).

vfs (fallback driver, inefficient).

✅ Check Current Storage Driver
docker info | grep Storage

✅ Show Disk Usage
docker system df

🔹 9. Clean Up Storage

Remove all stopped containers:

docker container prune


Remove unused images:

docker image prune


Remove unused volumes:

docker volume prune


Remove everything (⚠️ dangerous):

docker system prune -a --volumes

📌 Quick Summary

Volumes → Best for persistence.

Bind Mounts → Useful for dev (host ↔ container).

tmpfs → Fast, memory-only storage.

Storage Drivers → Control how images/layers are stored (default = overlay2).

Always use docker system df & prune commands to free space.
