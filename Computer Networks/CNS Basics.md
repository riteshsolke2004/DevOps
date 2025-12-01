🖧 Computer Networking for DevOps Engineers
1. What is Computer Network?
A computer network is a system where multiple computing devices (servers, laptops, IoT devices, etc.) communicate and share resources (data, applications, services).


🔹 Types of Networks:

LAN (Local Area Network) – Office, Home setup.

WAN (Wide Area Network) – The Internet.

MAN (Metropolitan Area Network) – City-wide ISP.

PAN (Personal Area Network) – Bluetooth, Hotspot.

🔹 Protocols in Networking: TCP/IP, HTTP, SSH, FTP.

👉 DevOps Relevance:

You’ll deploy apps on cloud networks (AWS, GCP, Azure).

Need to know how servers talk to each other (ports, firewalls, routing).

Helps in debugging infra issues (e.g., DB not reachable because of wrong subnet/firewall rule).

2. How Does Internet Work?

Devices connect via ISPs (Internet Service Providers).

Each device has an IP address (public/private).

Routing: Routers direct packets from source → destination using routing tables.

DNS resolves human-friendly names → IPs.

Data is sent as packets, using TCP/IP protocols.

👉 DevOps Relevance:

If your website is down, you must trace whether it’s DNS issue, routing issue, or firewall issue.

Understanding BGP routing failures (common cause of outages).

Debugging services like dig, nslookup, ping, curl.

3. Why Networking is Useful for DevOps?

DevOps = Automation + Infrastructure + Networking.

Without networking, you cannot:

Configure VPCs, subnets, firewalls, load balancers.

Set up CI/CD pipelines across regions.

Deploy apps securely with DNS, TLS, reverse proxies.

Debug downtime (DNS failures, port blocked, LB misconfig).

👉 Networking = Backbone of Cloud + DevOps.

4. OSI Model (7 Layers)

OSI is theoretical but important for troubleshooting.

Layer	Function	Examples	DevOps Angle
7. Application	User interaction	HTTP, DNS, FTP	Debugging APIs, web servers
6. Presentation	Data translation, encryption	SSL/TLS, JPEG, JSON	TLS certs, HTTPS config
5. Session	Session mgmt	NetBIOS, RPC	Stateful apps, sockets
4. Transport	Reliable delivery	TCP, UDP	Choosing TCP (web apps) vs UDP (streaming, DNS)
3. Network	Routing, addressing	IP, ICMP	VPC, Subnets, Routing tables
2. Data Link	MAC addressing, switching	Ethernet, ARP	Network switches, ARP cache
1. Physical	Transmission medium	Fiber, cables	Cloud abstracts this, but matters in on-prem

👉 DevOps Relevance:
When debugging, map the issue to the layer:

“Website not opening” → L7/L4 issue.

“IP unreachable” → L3 issue.

“Wrong MAC resolution” → L2 issue.

5. TCP/IP Model (Practical Model)

4 layers (more real-world than OSI).

Application → HTTP, DNS, SSH.

Transport → TCP (reliable), UDP (faster, no guarantee).

Internet → IP, ICMP.

Network Access → Ethernet, Wi-Fi.

👉 DevOps Relevance:

TCP vs UDP:

Use TCP for APIs, web servers.

Use UDP for DNS, video streaming.

Debugging with tools:

netstat -tulnp → Check listening TCP/UDP ports.

ss -lntup → Modern replacement.

6. IP / Subnets

IP = unique identifier of a machine.

Subnet = logical subdivision of a network.

CIDR Notation: 192.168.1.0/24 → 256 IPs.

👉 DevOps Relevance:

When deploying in AWS/GCP:

Create private subnets for DB.

Public subnet for load balancers.

Avoid IP conflicts in hybrid/multi-cloud.

Example:

Public Subnet → Internet-facing apps.

Private Subnet → Backend services.

7. DNS / NAT / Firewalls

DNS = translates domain → IP.

Tools: nslookup, dig.

NAT = maps private → public IP (so multiple devices share one public IP).

Firewalls = rules that allow/deny traffic (ports, IPs, protocols).

👉 DevOps Relevance:

DNS misconfig = most common outage cause.

You’ll configure AWS Route53, GCP Cloud DNS.

NAT Gateways in AWS for outbound internet access from private subnets.

Security Groups / NACLs are firewalls in cloud.

8. Ping / Traceroute / nslookup

Ping: Test reachability. (ping google.com)

Traceroute: Trace hops in the route. (traceroute google.com)

nslookup/dig: Check DNS records. (dig google.com A)

👉 DevOps Relevance:

Debugging why app not reachable:

Ping to test host reachability.

Traceroute to see where packet drops.

nslookup to confirm DNS resolution.

9. Load Balancers

Distribute traffic across multiple servers.

Types:

L4 (Transport) – distributes based on IP:Port.

L7 (Application) – distributes based on URL, headers.

👉 DevOps Relevance:

AWS ALB (Application Load Balancer), NLB (Network LB).

Scaling microservices & Kubernetes clusters.

Ensures high availability and fault tolerance.

10. VPC / VPC Peering

VPC = isolated cloud network where you deploy servers, DBs, etc.

Components:

Subnets (Public/Private).

Internet Gateway (IGW).

NAT Gateway.

Route Tables.

VPC Peering = connect two VPCs for internal traffic (without internet).

👉 DevOps Relevance:

In AWS: You’ll set up VPCs, configure route tables, attach gateways.

Peering is used when:

Your app in VPC-A needs DB in VPC-B.

Multi-region deployments.

Tools: AWS CLI, Terraform, CloudFormation for automation.
