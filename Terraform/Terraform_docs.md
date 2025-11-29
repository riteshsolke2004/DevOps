# 🌍 Terraform — Complete Notes & Setup Guide

## 📖 Table of Contents
1. History of Terraform
2. What is Terraform?
3. Why Terraform is Used (Advantages)
4. Core Concepts & Terminology
5. Terraform Architecture
6. Terraform Workflow (Lifecycle)
7. Providers & Modules
8. Real-World Applications
9. Installation & Setup (Windows / Linux / Mac)
10. First Terraform Project Example
11. Important Commands
12. Best Practices
13. Interview Questions (Quick Revision Notes)

---

## 🕰️ 1. History of Terraform
Terraform was created by **Mitchell Hashimoto and Armon Dadgar** from **HashiCorp**, first released in **July 2014**.  
It became popular for enabling Infrastructure-as-Code (IaC) across multiple cloud providers like AWS, Azure, GCP, Oracle Cloud, DigitalOcean, etc.

Terraform changed DevOps workflow by automating infrastructure provisioning using code instead of manual setup.

---

## 💡 2. What is Terraform?
Terraform is an **open-source Infrastructure as Code (IaC) tool** that allows developers and DevOps engineers to **define, provision, and manage cloud infrastructure using declarative configuration files**.

👉 We describe the desired infrastructure in `.tf` files and Terraform builds it automatically.

---

## 🎯 3. Why Terraform? (Key Advantages)
| Feature | Description |
|--------|------------|
| Multi-Cloud Support | Works with AWS, Azure, GCP, Kubernetes, VMware, DigitalOcean, etc. |
| Automation | Removes manual provisioning and reduces human errors |
| Infrastructure as Code | Infra becomes version-controlled like source code |
| Idempotency | Same code → same results everywhere |
| Execution Plan | Shows changes before applying |
| State Management | Tracks real infrastructure state |
| Reusable Modules | Reduces repetitive work |

---

## 🧠 4. Core Concepts
| Term | Meaning |
|------|---------|
| **Provider** | Cloud service connection (AWS, Azure, GCP, Docker, K8s etc.) |
| **Resource** | Component to create (VM, VPC, Database, S3 etc.) |
| **Variable** | Inputs used to configure |
| **Output** | Export values after provisioning |
| **Module** | Reusable package of Terraform code |
| **State File** | Tracks real infra state (`terraform.tfstate`) |
| **Plan** | Shows preview of changes |

---

## 🏗️ 5. Terraform Architecture
User -> Terraform Core -> Provider Plugins -> Cloud Platforms

## 🔄 6. Terraform Workflow / Lifecycle
Write Code (.tf files)

terraform init

terraform validate

terraform plan

terraform apply

terraform destroy (optional)



## 📦 7. Providers & Modules
### Providers Example

provider "aws" {
  region = "ap-south-1"
}
Module Example
hcl
Copy code
module "ec2_server" {
  source = "./ec2-module"
  instance_type = "t2.micro"
}

🧪 8. Real-World Use Cases

Deploy Virtual Machines automatically

Create Kubernetes clusters (EKS / AKS / GKE)

S3 buckets, Lambda, VPC, Load Balancers setup

Multi-environment setup (dev / stage / prod)

CI/CD infrastructure automation

Disaster recovery & backups

Automated scaling infra

🛠️ 9. Installation & Setup
Windows Setup
1. Download Terraform zip:
   https://developer.hashicorp.com/terraform/downloads

2. Extract zip to:
   C:\Program Files\Terraform

3. Add the path to Environment Variables (PATH)
4. Verify installation:
   terraform -version

Linux / Ubuntu Setup
sudo apt update
sudo apt install -y gnupg software-properties-common
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/hashicorp.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt install terraform
terraform -version


🚀 10. First Terraform Example

Create a folder and file:
main.tf

provider "aws" {
  region = "ap-south-1"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "terraform-demo-bucket-1234"
}

Run Commands
terraform init
terraform plan
terraform apply
terraform destroy

🔥 11. Important Commands
Command	Description
terraform init	Download providers & initialize
terraform validate	Validate code
terraform plan	Show preview
terraform apply	Create infra
terraform destroy	Delete infra
terraform fmt	Format code
terraform show	Show state
terraform state list	List resources
terraform output	Prints output values
📏 12. Best Practices

Always use version control (GitHub / GitLab / Bitbucket)

Maintain environments: dev / stage / prod separately

Never push terraform.tfstate to GitHub

Use remote state (S3 + DynamoDB lock)

Always review terraform plan before apply

##📚 13. Interview Quick Notes

Terraform supports declarative approach

Maintains state to detect changes

Supports immutable infrastructure

Multi-cloud IaC tool

Competes with CloudFormation, Pulumi, Ansible

💬 Conclusion

Terraform is a foundational DevOps tool to automate infrastructure provisioning.
Mastering Terraform = strong DevOps + Cloud + automation power 👊🔥
