# Multi-Cloud Linux Deployment – AWS and Microsoft Azure

## Project Overview

This hands-on project demonstrates how to deploy the same custom HTML website on Linux virtual machines in two different cloud platforms: **Amazon Web Services (AWS)** and **Microsoft Azure**.

We provisioned Ubuntu Linux virtual machines, configured secure SSH access, installed NGINX, deployed a custom website, and verified public HTTP access on both platforms.

The project also explores the similarities and differences between AWS EC2 and Azure Virtual Machines.

## Project Objectives

- Provision Linux virtual machines on AWS and Microsoft Azure.
- Establish secure SSH access using key-based authentication.
- Configure inbound network rules for SSH and HTTP.
- Perform basic Linux system administration.
- Install and configure the NGINX web server.
- Deploy the same custom HTML dashboard on both VMs.
- Verify local and public website access.
- Compare the infrastructure and networking configurations of both cloud providers.

## Architecture

```text
                 User / Web Browser
                         |
                HTTP – Port 80
                         |
             +-----------+-----------+
             |                       |
        AWS Cloud               Microsoft Azure
             |                       |
        AWS EC2 VM              Azure Linux VM
        Ubuntu Linux            Ubuntu Linux
             |                       |
          NGINX                   NGINX
             |                       |
       Custom HTML              Custom HTML
        Dashboard                Dashboard
```

Both virtual machines host the same custom website independently.

## Cloud Infrastructure

| Configuration | AWS | Microsoft Azure |
|---|---|---|
| Cloud service | Amazon EC2 | Azure Virtual Machines |
| Region | US West (Oregon) | West US 2 |
| VM name | multicloud-aws-vm | multicloud-azure-vm |
| Operating system | Ubuntu 26.04.1 LTS | Ubuntu 24.04.5 LTS |
| VM size | t3.micro | Standard_B2ats_v2 |
| vCPU | 2 | 1 |
| Memory | 1 GiB configured | Approximately 887 MiB reported |
| Storage | 8 GiB gp3 EBS | Standard SSD |
| Web server | NGINX | NGINX |

## Security and Network Configuration

Both cloud environments were configured to allow the required network traffic.

| Configuration | AWS | Azure |
|---|---|---|
| Network security | Security Group | Network Security Group (NSG) |
| SSH | TCP 22, restricted to client public IP (/32) | TCP 22, restricted to client public IP (/32) |
| HTTP | TCP 80, public access | TCP 80, public access |
| Authentication | SSH key pair | SSH key authentication |

SSH access was restricted to the client's public IP address, while HTTP access was enabled for public website verification.

## Deployment Activities

### 1. AWS EC2 Deployment

- Provisioned an Ubuntu Linux EC2 instance.
- Configured Security Group inbound rules.
- Connected to the instance using SSH.
- Performed Linux system checks.
- Updated package information.
- Installed and started NGINX.
- Deployed the custom HTML dashboard.
- Verified the website locally and through its public endpoint.

### 2. Microsoft Azure VM Deployment

- Created an Ubuntu Linux virtual machine.
- Configured the Network Security Group.
- Connected using SSH key authentication.
- Performed Linux system checks.
- Updated package information.
- Installed and started NGINX.
- Deployed the same custom HTML dashboard.
- Verified the website locally and through its public endpoint.

## Verification

The following checks were completed on both virtual machines:

- SSH connectivity.
- Operating system and kernel information.
- CPU and memory information.
- Disk usage.
- NGINX service status.
- Local HTTP response using `curl`.
- Public website accessibility through a browser.

The local HTTP test returned `HTTP/1.1 200 OK`, and the custom dashboard was accessible through both public endpoints.

## Key Learnings

Through this project, we gained practical experience with:

- AWS EC2 and Microsoft Azure Virtual Machines.
- Linux administration and command-line operations.
- SSH key-based authentication.
- AWS Security Groups and Azure NSGs.
- NGINX installation and service management.
- Static website deployment.
- HTTP connectivity and troubleshooting.
- Comparing cloud infrastructure services.

## Project Progress

| Milestone | Status |
|---|---|
| AWS EC2 Infrastructure | Completed |
| AWS Linux Administration | Completed |
| AWS NGINX Installation | Completed |
| AWS Custom Website Deployment | Completed |
| Azure Linux VM Provisioning | Completed |
| Azure Linux Administration and NGINX | Completed |
| Azure Custom Website Deployment | Completed |
| Azure Public Website Verification | Completed |
| Multi-Cloud Comparison | In Progress |
| Resource Cleanup and Cost Review | Upcoming |

## Repository Structure

```text
multi-cloud-linux-deployment/
│
├── README.md
│
├── screenshots/
│
└── docs/
```

## Cost Management

This project uses cloud resources for hands-on learning. Resource cleanup and cost review will be performed after the deployment and comparison activities.

Stopping a VM alone does not necessarily remove all associated cloud charges. Associated resources will also be reviewed during cleanup.

## Author

**Malathi Shetty**

GitHub: [@withmalathi](https://github.com/withmalathi)

---

*This project documents a practical learning journey deploying and verifying the
