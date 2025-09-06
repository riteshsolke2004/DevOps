# 🖥️ Linux Networking & DevOps Commands Cheat Sheet

A quick reference for commonly used **Linux networking and DevOps commands** with simple explanations and examples.  
Perfect for **DevOps, Networking, Cloud, and TCS CodeVita prep** 🚀  

---

## 🔹 Basic Network Commands

### `ping`
- **Use:** Test connectivity to a host.  
- **Example:**  
  ```bash
  ping google.com

traceroute

Use: Shows the path packets take to reach a host.

Example:

traceroute google.com

nslookup

Use: Query DNS to get domain → IP mapping.

Example:

nslookup google.com

dig

Use: Advanced DNS lookup with detailed info.

Example:

dig google.com

🔹 Network Interfaces
ifconfig

Use: Show or configure network interfaces (legacy).

Example:

ifconfig

iwconfig

Use: Configure wireless interfaces (Wi-Fi).

Example:

iwconfig

ifplugstatus

Use: Check if Ethernet cable is connected.

Example:

ifplugstatus eth0

🔹 Host & IP
hostname

Use: Show or set system hostname.

Example:

hostname

ip

Use: Modern command for IP, routing, and devices.

Examples:

ip addr show       # Show IP addresses
ip route show      # Show routing table
ip link show       # Show interfaces

route

Use: Display or modify routing table (legacy, replaced by ip route).

Example:

route -n

🔹 Connections & Ports
netstat

Use: Show active connections and listening ports.

Example:

netstat -tulnp

ss

Use: Modern replacement for netstat.

Example:

ss -tulnp

telnet

Use: Connect to remote host (test open ports).

Example:

telnet google.com 80

nc (Netcat)

Use: Debug, test ports, transfer data.

Examples:

nc -zv google.com 80           # Test port
nc -l 1234 > file.txt          # Listen & receive
nc 192.168.1.10 1234 < file.txt  # Send file

nmap

Use: Network scanner to detect hosts, open ports, and services.

Example:

nmap 192.168.1.1

🔹 ARP & Mapping
arp

Use: Show IP ↔ MAC address mapping.

Example:

arp -a

🔹 Downloading Tools
curl

Use: Send requests and transfer data (great for APIs).

Examples:

curl -I https://google.com   # Get headers
curl -O https://example.com/file.txt  # Download file

wget

Use: Download files from the internet.

Examples:

wget https://example.com/file.txt
wget -r https://example.com   # Recursive download

🔹 Monitoring & Utilities
watch

Use: Run a command repeatedly at intervals.

Examples:

watch -n 2 df -h     # Monitor disk usage
watch ps aux         # Monitor processes

whois

Use: Get domain registration details.

Example:

whois google.com

🔹 Firewall & Security
iptables

Use: Configure firewall rules (packet filtering).

Examples:

sudo iptables -L -v                      # List rules
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT  # Allow SSH
sudo iptables -A INPUT -s 192.168.1.100 -j DROP     # Block IP
