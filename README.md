**1. Project Overview**
This project demonstrates automated deployment of a containerized web application on AWS using Infrastructure-as-Code (IaC) and Continuous Integration/Continuous Delivery (CI/CD).
**COMPONENT: TECHNOLOGY : DETAILS**
Cloud Platform: AWSRegion : eu-west-1 (Ireland)
IaC Tool:Terraform (>= v1.6.0): Provisions 8 AWS resources
Containerization: Docker : nginx:alpine base image, Port 80
CI/CD : GitHub Actions : Automates build, push, and deployment
Application : Web Server : Image: msd2706/aws-project-app:latest

**2. Infrastructure Details (Terraform)**

**Networking Configuration**

VPC CIDR: 10.0.0.0/16 (65,536 IP addresses)
Public Subnet: 10.0.1.0/24 in availability zone eu-west-1a
Internet Gateway: Enables public internet access
Route Table: Routes all traffic (0.0.0.0/0) to Internet Gateway

**EC2 Instance**

AMI: ami-0bc691261a82b32bc (Ubuntu Server)
Instance Type: t3.micro (2 vCPU, 1 GB RAM)
SSH Key: Ed25519 key pair from ~/.ssh/id_ed25519.pub
User Data Script: Installs Docker on launch


3. Docker Containerization 
Dockerfile:
FROM nginx:alpine
COPY --chown=nginx:nginx ./app/ /usr/share/nginx/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

**4. CI/CD Pipeline (GitHub Actions) 
GitHub Secrets Required**

DOCKER_USERNAME - Docker Hub username
DOCKER_PASSWORD - Docker Hub password
SSH_PRIVATE_KEY - Complete Ed25519 private key with headers
EC2_PUBLIC_IP - EC2 instance public IP address

**Automated Workflow**
When code is pushed to main branch:

Build Stage: Checks out code, builds Docker image, pushes to Docker Hub
Deploy Stage: Connects to EC2 via SSH, pulls latest image, replaces running container


**5. Security Implementation **

IAM Users: Created IAM user with AdministratorAccess instead of using root account
SSH Keys: Ed25519 authentication (more secure than passwords)
Non-root Containers: Docker runs as nginx user, limiting compromise impact
GitHub Secrets: Encrypted credential storage
Security Groups: Firewall rules control network access

**6. Deployment Process** 

# Initialize and deploy infrastructure
terraform init
terraform apply

# Deploy application (automated via GitHub Actions)
git add .
git commit -m "Update application"
git push origin main




