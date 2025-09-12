---
## Quick intro: Jenkins + CI/CD (1-paragraph)
- **Jenkins** is an automation server used to build, test and deploy code. In CI/CD, Jenkins automates the pipeline steps so teams catch issues fast and ship reliably. Think: commit → build → test → artifact → deploy. Jenkins can run freestyle jobs, scripted or declarative pipelines, and scale via agents (nodes).

---
## 1) Jenkins Setup on VM (AWS EC2) — Full Steps
**Goal:** Launch an EC2 machine and run Jenkins accessible on port `8080`.

### 1.1 Launching EC2 (Ubuntu) — quick checklist
1. AWS Console → EC2 → Launch Instance → Select **Ubuntu 22.04 LTS** (or 20.04).
2. Instance type: `t2.micro` (for practice) or `t3.medium` (real work).
3. Storage: 20GB default is ok. Increase if you will build containers/artifacts.
4. Security Group inbound rules: Allow `SSH (22)` from your IP, `HTTP (80)` optional, `Custom TCP 8080` from your IP or 0.0.0.0/0 (if public), and `TCP 50000` (for JNLP agents) if you will use agents over JNLP.
5. (Optional) Elastic IP for a stable public IP.

### 1.2 SSH into instance
```bash
ssh -i path/to/key.pem ubuntu@<EC2_PUBLIC_IP>
sudo apt update && sudo apt upgrade -y
```

### 1.3 Install Java (Jenkins needs Java)
```bash
sudo apt install -y openjdk-11-jdk
java -version   # confirm JDK 11+
```

### 1.4 Install Jenkins (Debian/Ubuntu method)
```bash
# add jenkins key & repo
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install -y jenkins

# start & enable
sudo systemctl enable --now jenkins
sudo systemctl status jenkins
```

### 1.5 Firewall / Security Group
- If `ufw` is enabled:
```bash
sudo ufw allow OpenSSH
sudo ufw allow 8080/tcp
sudo ufw enable
```
- Ensure AWS Security Group has inbound `8080` allowed.

### 1.6 Unlock Jenkins & initial setup
- Get initial admin password:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
- Open browser: `http://<EC2_PUBLIC_IP>:8080` and paste the password.
- Install suggested plugins (recommended) or choose manually.
- Create first admin user.

**Plugins to install now:** Git, GitHub, Pipeline, Blue Ocean, Credentials Binding, Docker Pipeline, Maven Integration, Role-based Authorization Strategy, SSH Build Agents (or “SSH Slaves”), Multibranch Pipeline.
