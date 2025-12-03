# 🚀 GitLab – Complete Notes & Installation Guide

GitLab isn’t just a code-hosting platform.  
It’s a **full DevOps powerhouse** where you can plan, build, test, deploy, and monitor your software in one place.  
A whole ecosystem — not just a repository.

---

## 🧠 What is GitLab?
GitLab is a **web-based DevOps lifecycle platform** that provides:
- Source code management (like GitHub)
- Built-in CI/CD pipelines
- Issue tracking & project management
- Container registry & package registry
- Self-hosting support (private GitLab server)

Basically — **GitHub + Jenkins + Jira + Docker Registry → All in one tool.**

---

## 🔥 Why Do We Use GitLab?
| Reason | Explanation |
|--------|------------|
| Built-in CI/CD | No need for Jenkins or other CI tools |
| Self-Hosted Option | Run GitLab on your own server for privacy |
| Enterprise Ready | Used for large-scale organizations |
| DevOps Automation | Plan → Code → Test → Deploy → Monitor |
| Security & Compliance | Built-in SAST, Dependency scanning |
| Better permissions & access control | For enterprise teams |

---

## 🆚 GitLab vs GitHub — Key Differences

| Feature | GitHub | GitLab |
|---------|--------|--------|
| CI/CD | Requires **GitHub Actions** | **Built-in CI/CD** (core feature) |
| Hosting | Mainly cloud | Cloud + **Self hosted option** |
| Project Management | Basic | **Agile Boards, Epics, Roadmaps** |
| Private Repos | Paid earlier, now free | Free |
| DevOps Support | Needs integrations | **All-in-one platform** |
| Container Registry | Add-on | **Built-in** |

🤖 GitLab CI/CD Example

Create .gitlab-ci.yml in root:

stages:
  - build
  - deploy

build_job:
  stage: build
  script:
    - echo "Building the project"

deploy_job:
  stage: deploy
  script:
    - echo "Deploying application"


Pipeline runs automatically after push 💥

💡 Important GitLab Features to Know
Feature	Description
GitLab CI/CD	Automate build & deploy
Container Registry	Stores Docker images
Issue Boards	Agile task management
Runner	Executes pipeline jobs
Auto DevOps	Auto builds + tests + deploys
🧠 GitLab Runner Installation (for CI/CD)
sudo apt update
sudo apt install gitlab-runner -y


Register Runner:

gitlab-runner register


Enter values:

URL: http://your-server-ip/
Token: from project -> Settings -> CI/CD -> Runners

Start Runner:

sudo gitlab-runner start

🎯 Best Use Cases of GitLab

Private enterprise level projects

Complete CI/CD pipeline automation

Deploy apps to AWS, GCP, Azure, Kubernetes

Manage microservices with Docker & Helm

Secure DevOps pipeline with integrated security scans

**Conclusion:**  
GitHub → best for open-source + community  
GitLab → best for enterprise + DevOps automation

---

# 🛠 Installation Guide – GitLab Self-Hosted Version

## 📍 Step 1: Install Dependencies
```bash
sudo apt update
sudo apt install -y curl openssh-server ca-certificates tzdata perl
sudo apt install -y postfix  # For email notifications


📍 Step 2: Add GitLab Package Repository
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash

📍 Step 3: Install GitLab
sudo EXTERNAL_URL="http://your-server-ip" apt install gitlab-ee

📍 Step 4: Reconfigure GitLab
sudo gitlab-ctl reconfigure


🔑 Step 5: Login & Set Password

Get initial root password:

sudo cat /etc/gitlab/initial_root_password


Open browser:

http://your-server-ip


Login:

username: root
password: (copied one)


🥳 GitLab server is now ready.

🧪 Test Repository Creation
git clone http://your-server-ip/username/repo.git
cd repo
touch readme.md
git add .
git commit -m "first commit"
git push origin main
