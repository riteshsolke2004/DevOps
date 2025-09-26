# 🌐 Docker Networking – In Depth

Docker networking allows containers to communicate with each other, the host system, and the outside world.  
By default, Docker creates an isolated network environment for containers.

---

## 🔹 Basics of Docker Networking
- Each container gets its **own network namespace**.
- Containers can be connected to one or more networks.
- Networking is handled by the **Docker daemon**.
- Uses **Linux kernel features** like namespaces, veth pairs, and iptables.

---

## 🔹 Types of Docker Networks

### 1. Bridge Network (Default)
- Created automatically when Docker is installed.
- If no network is specified, containers attach to `bridge`.
- Allows inter-container communication on the same bridge.
- Containers can be accessed using **container names** as DNS.

Example:
```bash
# List networks
docker network ls

# Run container in bridge network
docker run -d --name web --network bridge nginx

# Inspect bridge network
docker network inspect bridge


2. Host Network

Removes network isolation between the container and host.

Container shares the host’s network stack (IP, ports).

Useful for high-performance networking.
docker run -d --network host nginx

3. None Network

Completely isolates the container from any network.

Container has no internet access.

Useful for security-sensitive workloads.
docker run -d --network none nginx


4. User-Defined Bridge Network

Provides better control and flexibility than the default bridge.

Containers in the same custom bridge can resolve each other by name.

Recommended for multi-container applications.
# Create a custom network
docker network create mynetwork

# Run containers in this network
docker run -d --name app1 --network mynetwork nginx
docker run -d --name app2 --network mynetwork alpine sleep 3600

# Test connectivity
docker exec -it app2 ping app1


5. Overlay Network

Used for multi-host Docker Swarm deployments.

Enables containers on different Docker hosts to communicate securely.

Requires a key-value store (e.g., etcd, Consul, built-in swarm mode).

Used in orchestration (Docker Swarm, Kubernetes).
# In Docker Swarm
docker network create --driver overlay myoverlay


6. Macvlan Network

Assigns a MAC address to containers, making them appear as physical devices.

Containers connect directly to the host’s physical network.

Useful for legacy apps that require a unique MAC/IP.

Example:

docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 mymacvlan

Docker Networking CLI Commands

# List all networks
docker network ls

# Inspect network details
docker network inspect <network_name>

# Connect a container to a network
docker network connect <network_name> <container_id>

# Disconnect a container from a network
docker network disconnect <network_name> <container_id>

# Remove a network
docker network rm <network_name>


🔹 DNS in Docker

Docker has an embedded DNS server.

Containers in the same user-defined bridge can resolve each other by name.

Example:

ping app1 from app2 works if both are in the same custom network.

🔹 Networking with Docker Compose

Define multiple networks in docker-compose.yml.

Example:

version: "3"
services:
  web:
    image: nginx
    networks:
      - frontend
  db:
    image: mysql
    networks:
      - backend

networks:
  frontend:
  backend:

🔹 Port Mapping

Containers are isolated from the host by default.

To expose ports:

docker run -d -p 8080:80 nginx


8080:80 → Maps host port 8080 to container port 80.

🔹 Security Considerations

Use user-defined networks for isolation.

Limit port exposure with -p only when necessary.

Use none network for isolated workloads.

For sensitive workloads, prefer overlay with encryption.

✅ Summary

Bridge: Default, containers talk internally.

Host: Shares host network (no isolation).

None: No network access.

User-defined bridge: Better isolation + DNS.

Overlay: Multi-host networking (Swarm/Kubernetes).

Macvlan: Direct access to physical network.
