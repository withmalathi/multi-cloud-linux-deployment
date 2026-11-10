# Multi-Cloud Linux Deployment – AWS and Microsoft Azure

A hands-on cloud project demonstrating how to deploy the same custom HTML website on Ubuntu Linux virtual machines in **Amazon Web Services (AWS)** and **Microsoft Azure**, using NGINX.

---

## Project Overview

The goal of this project was to deploy and verify the same website independently on two different cloud platforms.

We provisioned Linux virtual machines, configured secure SSH connectivity, installed NGINX, deployed our custom HTML dashboard, and verified website accessibility through public endpoints.

This project provided practical experience with cloud infrastructure, Linux administration, networking, web server deployment, and multi-cloud comparison.

## Architecture

The same custom website is hosted independently on both cloud providers.

```text
                    User / Web Browser
                            |
                       HTTP : 80
                            |
              +-------------+-------------+
              |                           |
          AWS Cloud                 Microsoft Azure
              |                           |
           AWS EC2                  Azure Virtual Machine
              |                           |
         Ubuntu Linux                Ubuntu Linux
              |                           |
            NGINX                       NGINX
              |                           |
       Custom HTML Website        Custom HTML Website
```

## Technologies and Services

| Category | Technologies |
|---|---|
| Cloud platforms | AWS, Microsoft Azure |
| Compute | Amazon EC2, Azure Virtual Machines |
| Operating system | Ubuntu Linux |
| Web server | NGINX |
| Website | HTML and CSS |
| Remote access | SSH |
| Networking security | AWS Security Groups, Azure NSGs |
| Verification | Linux commands, `curl`, web browser |
| Version control | Git and GitHub |

## Cloud Infrastructure Comparison

| Configuration | AWS | Microsoft Azure |
|---|---|---|
| Cloud service | Amazon EC2 | Azure Virtual Machine |
| Region | US West (Oregon) | West US 2 |
| VM name | `multicloud-aws-vm` | `multicloud-azure-vm` |
| Operating system | Ubuntu 26.04.1 LTS | Ubuntu 24.04.5 LTS |
| VM size | `t3.micro` | `Standard_B2ats_v2` |
| vCPU | 2 | 1 |
| Configured / reported memory | 1 GiB configured | Approximately 887 MiB reported |
| Storage | 8 GiB gp3 EBS | Standard SSD |
| Web server | NGINX | NGINX |

## Deployment Evidence

### 1. Virtual Machine Configuration

The following comparison image highlights the compute configuration on both cloud platforms.

![AWS and Azure VM configuration](comparison-aws-azure/aws-azure-vm-configuration.png)

### 2. Network Security Configuration

Both environments were configured with the required inbound rules:

- SSH – TCP port 22, restricted to the client's public IP address using `/32`.
- HTTP – TCP port 80, allowed for public website access.

AWS uses Security Groups, while Azure uses Network Security Groups (NSGs).

![AWS and Azure network security comparison](comparison-aws-azure/aws-azure-network-security.png)

### 3. Website Deployment

The same custom HTML dashboard was deployed on both Linux virtual machines using NGINX.

![AWS and Azure website deployment comparison](comparison-aws-azure/aws-azure-public-website-deployment.png)

### 4. AWS Website

![Custom website deployed on AWS](screenshots/aws-screenshots/aws-website.png)

### 5. Azure Website

![Custom website deployed on Azure](screenshots/az-screenshots/azure-completed-public-website.png)

## Deployment Process

### Phase 1 – AWS EC2 Deployment

1. Created an Ubuntu EC2 instance in the US West (Oregon) region.
2. Configured the EC2 instance and storage.
3. Created and configured an SSH key pair.
4. Configured the Security Group to allow SSH and HTTP traffic.
5. Connected to the instance using SSH.
6. Verified Linux system information, memory, CPU, and disk usage.
7. Updated package information.
8. Installed NGINX.
9. Verified that the NGINX service was active and running.
10. Tested the web server locally using `curl`.
11. Deployed the custom HTML dashboard.
12. Verified the website through the public endpoint.

### Phase 2 – Microsoft Azure Linux VM Deployment

1. Created a resource group in Azure.
2. Provisioned an Ubuntu Linux virtual machine in West US 2.
3. Configured the VM size, operating system disk, and networking.
4. Configured SSH key-based authentication.
5. Configured NSG inbound rules for SSH and HTTP.
6. Connected to the VM using SSH.
7. Verified Linux system information, memory, CPU, and disk usage.
8. Updated Ubuntu package information.
9. Installed NGINX.
10. Verified that the NGINX service was active and running.
11. Tested the web server locally using `curl`.
12. Deployed the same custom HTML dashboard.
13. Verified the website through the public endpoint.

## Verification Commands

The following Linux commands were used during the project.

| Command | Purpose |
|---|---|
| `whoami` | Verify the logged-in Linux user |
| `hostname` | Check the VM hostname |
| `cat /etc/os-release` | Identify the Linux distribution and version |
| `uname -r` | Check the running kernel version |
| `nproc` | Check the number of available CPU cores |
| `free -h` | Inspect memory usage |
| `df -h` | Inspect disk usage |
| `sudo systemctl status nginx` | Check NGINX service status |
| `curl -I http://localhost` | Verify the local HTTP response |
| `curl -s http://localhost` | Inspect locally served website content |

The local HTTP test returned `HTTP/1.1 200 OK` on both virtual machines. The custom website was also verified through both public endpoints.

## AWS and Azure – Similarities and Differences

### Similarities

- Both platforms provide cloud-based Linux virtual machines.
- Both support SSH key-based remote access.
- Both provide network security rules to control inbound traffic.
- Both can host the same NGINX web server.
- Both successfully served the same custom HTML website.

### Differences

- AWS uses EC2, while Azure provides Virtual Machines.
- AWS uses Security Groups, while Azure uses Network Security Groups.
- The VM sizing and naming conventions differ.
- Storage services and configuration options differ between providers.
- The cloud portals and resource management workflows are different.

## Key Learnings

Through this hands-on project, we practised:

- Provisioning Linux infrastructure on two cloud platforms.
- Connecting to remote Linux VMs using SSH.
- Understanding inbound network security rules.
- Performing basic Linux system administration.
- Installing and managing NGINX.
- Deploying a custom static website.
- Testing local and public HTTP connectivity.
- Comparing AWS and Azure infrastructure.

## Project Milestones

| # | Milestone | Status |
|---|---|---|
| 01 | AWS EC2 infrastructure provisioning | Completed |
| 02 | AWS Linux administration | Completed |
| 03 | AWS NGINX installation | Completed |
| 04 | AWS custom website deployment | Completed |
| 05 | Azure Linux VM provisioning | Completed |
| 06 | Azure Linux administration and NGINX installation | Completed |
| 07 | Azure custom website deployment | Completed |
| 08 | Azure public website verification | Completed |
| 09 | Multi-cloud comparison | In Progress |
| 10 | Resource cleanup and cost review | Upcoming |

## Repository Structure

```text
multi-cloud-linux-deployment/
│
├── README.md
│
├── comparison-aws-azure/
│   ├── aws-azure-network-security.png
│   ├── aws-azure-public-website-deployment.png
│   └── aws-azure-vm-configuration.png
│
├── docs/
│
├── screenshots/
│   ├── aws-screenshots/
│   └── az-screenshots/
│
└── website-pages-aws-azure/
    ├── aws-website-version-1.html
    ├── aws-website-version-2.html
    ├── azure-website-version-1.html
    └── azure-website-version-2.html
```

## Screenshots and Documentation

- [AWS deployment screenshots](screenshots/aws-screenshots/)
- [Azure deployment screenshots](screenshots/az-screenshots/)
- [AWS and Azure comparison images](comparison-aws-azure/)
- [Website HTML files](website-pages-aws-azure/)
- [Project documentation](docs/)

## Cost Management

Cloud resources are used for hands-on learning. Resource cleanup and cost review are planned as the final project milestone.

Stopping a virtual machine does not necessarily remove all associated charges. Disks, public IP addresses, and other retained resources must also be reviewed.

## Author

**Malathi Shetty**

GitHub: [@withmalathi](https://github.com/withmalathi)

Repository: [multi-cloud-linux-deployment](https://github.com/withmalathi/multi-cloud-linux-deployment)

---

*Building practical cloud skills through hands-on deployment, verification, and comparison across AWS and Microsoft Azure.*