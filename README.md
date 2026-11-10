# Multi-Cloud Linux Deployment – AWS and Microsoft Azure

A hands-on cloud project demonstrating how to provision Ubuntu Linux virtual machines on **Amazon Web Services (AWS)** and **Microsoft Azure**, install NGINX, deploy the same custom HTML website, and verify public accessibility.

---

# 1. Problem Statement

In today's digital environment, businesses need reliable infrastructure to host websites and applications. Cloud platforms provide virtual machines, networking, storage, and security services that make it possible to deploy applications without maintaining physical servers.

However, beginners often find it difficult to understand how cloud infrastructure works in practice. They need hands-on experience with virtual machine provisioning, SSH connectivity, Linux administration, network security, and web server deployment.

This project addresses that learning gap by deploying the same custom HTML website independently on AWS and Microsoft Azure.

We provision Ubuntu Linux virtual machines, configure secure remote access, install NGINX, deploy the website, and verify that it is accessible through both cloud providers' public endpoints.

# 2. Why We Need This Use Case

This use case helps us understand the practical steps involved in hosting a website on cloud infrastructure.

Through this project, we learn how to:

- Provision and configure Linux virtual machines on AWS and Azure.
- Establish secure remote connectivity using SSH key authentication.
- Configure inbound network rules for SSH and HTTP traffic.
- Perform basic Linux system administration.
- Install and manage the NGINX web server.
- Deploy a custom HTML website.
- Verify local and public website connectivity.
- Compare infrastructure and networking configurations across two cloud platforms.

# 3. When We Need This Use Case

This type of deployment is useful when:

- Hosting a static website on a cloud virtual machine.
- Learning cloud infrastructure and Linux administration.
- Setting up a development or testing environment.
- Understanding how web applications are exposed through public endpoints.
- Comparing cloud services and deployment workflows across providers.
- Practising infrastructure deployment before moving to more advanced DevOps automation.

# 4. Prerequisites

Before starting the lab, ensure that we have:

| Requirement | Purpose |
|---|---|
| AWS account | To create an EC2 virtual machine |
| Microsoft Azure account | To create an Azure Linux VM |
| Windows computer | Local machine for managing the deployment |
| WSL Ubuntu or Git Bash | Terminal access and SSH commands |
| SSH client | To connect to remote Linux VMs |
| Git | To clone and manage the project repository |
| GitHub account | To store project files and screenshots |
| Basic Linux knowledge | To execute administration commands |
| Basic HTML knowledge | To understand the deployed website |

**Project repository:**

https://github.com/withmalathi/multi-cloud-linux-deployment

Clone the repository in the WSL Ubuntu terminal:

```bash
git clone https://github.com/withmalathi/multi-cloud-linux-deployment.git
```

Move into the project directory:

```bash
cd multi-cloud-linux-deployment
```

Verify the project files:

```bash
ls -la
```

Expected folders include:

```text
comparison-aws-azure/
docs/
screenshots/
website-pages-aws-azure/
README.md
```

**Important:** Create your own SSH key pairs during the lab. Never upload private SSH keys to GitHub or share them with anyone.

# 5. Advantages and Disadvantages

## Advantages

- **Cloud flexibility:** We can provision Linux virtual machines without maintaining physical servers.
- **Practical learning:** We gain hands-on experience with cloud infrastructure and Linux.
- **Portability:** The same static website can be deployed on different cloud providers.
- **Security awareness:** We learn to restrict SSH access and configure HTTP access.
- **Comparison:** We understand similarities and differences between AWS and Azure services.

## Disadvantages

- **Initial configuration:** Beginners need to understand virtual machines, networking, SSH and security rules.
- **Cost management:** Cloud resources may incur charges while running or after being stopped.
- **Security responsibility:** SSH access, firewall rules, credentials and software updates must be managed carefully.
- **Platform differences:** AWS and Azure use different terminology, portals and resource configuration workflows.

# 6. Step-by-Step Implementation

We will complete the project in four phases:

1. AWS EC2 deployment.
2. Microsoft Azure Linux VM deployment.
3. Multi-cloud comparison and verification.
4. Resource cleanup and cost review.

---

# Phase 1 – AWS EC2 Deployment

## Step 1.1 – Launch an Ubuntu EC2 Instance

**What are we doing?**

We are creating a Linux virtual machine using Amazon EC2.

**Why are we doing this?**

An EC2 instance provides the compute environment on which we will install NGINX and host our website.

**How do we do it?**

1. Sign in to the AWS Management Console.
2. Open the EC2 service.
3. Select **Launch instance**.
4. Enter the instance name:

   `multicloud-aws-vm`

5. Under Application and OS Images, select Ubuntu Server 26.04.1 LTS (64-bit x86).
6. Select the instance type:

   `t3.micro`

7. Create a new key pair:
   - Key pair name: `multicloud-aws-key`
   - Key pair type: RSA
   - Private key file format: PEM
8. Download and securely store the private key file.
9. Configure the network and security group.
10. Configure inbound rules:
    - SSH – TCP port 22 – Source: My IP.
    - HTTP – TCP port 80 – Source: Anywhere IPv4 (`0.0.0.0/0`).
11. Configure the root storage:
    - Size: 8 GiB
    - Volume type: gp3
12. Review the configuration and select **Launch instance**.

**Expected result:**

The EC2 instance should reach the **Running** state, and its instance status checks should pass.

Our lab configuration:

| Setting | Value |
|---|---|
| Instance name | `multicloud-aws-vm` |
| Region | US West (Oregon) – `us-west-2` |
| Operating system | Ubuntu 26.04.1 LTS |
| Instance type | `t3.micro` |
| vCPU | 2 |
| Configured memory | 1 GiB |
| Root storage | 8 GiB gp3 |
| SSH key pair | `multicloud-aws-key` |

## Step 1.2 – Connect to the EC2 Instance Using SSH

**What are we doing?**

We are connecting from our local computer to the remote Ubuntu virtual machine.

**Why are we doing this?**

SSH provides secure command-line access so we can administer the Linux server remotely.

**How do we do it?**

Open the WSL Ubuntu terminal.

If our PEM key is stored in the Windows D: drive under `D:\pem`, its WSL path is `/mnt/d/pem`.

Set the private key permissions:

```bash
chmod 400 /mnt/d/pem/multicloud-aws-key.pem
```

Connect to the EC2 instance:

```bash
ssh -i /mnt/d/pem/multicloud-aws-key.pem ubuntu@<AWS_PUBLIC_IP>
```

Replace `<AWS_PUBLIC_IP>` with the public IPv4 address shown in the EC2 console.

**Expected result:**

We should successfully connect to the Ubuntu instance and see a Linux terminal prompt.

Verify the logged-in user:

```bash
whoami
```

Expected output:

```text
ubuntu
```

## Step 1.3 – Verify Linux System Information

Before installing the web server, we will inspect the Linux environment.

Run the following commands:

**Check the hostname:**

```bash
hostname
```

**Check the operating system:**

```bash
cat /etc/os-release
```

**Check the running kernel:**

```bash
uname -r
```

**Check the number of CPUs:**

```bash
nproc
```

**Check memory usage:**

```bash
free -h
```

**Check disk usage:**

```bash
df -h
```

**Why are we doing this?**

These commands help us understand the operating system and the compute resources available on our virtual machine.

**Expected result:**

We should see the Ubuntu operating system details, CPU count, memory information and available disk space.

## Step 1.4 – Update Ubuntu Package Information

**What are we doing?**

We are refreshing the package information available to Ubuntu.

**Why are we doing this?**

Before installing software, we should ensure that the package manager has the latest available package information.

Run:

```bash
sudo apt update
```

**Expected result:**

Ubuntu should retrieve package information from its configured repositories and complete the update successfully.

## Step 1.5 – Install NGINX

**What are we doing?**

We are installing NGINX on the Ubuntu virtual machine.

**Why are we doing this?**

NGINX will act as our web server and serve the custom HTML website over HTTP.

Run:

```bash
sudo apt install nginx -y
```

Check the NGINX service:

```bash
sudo systemctl status nginx --no-pager
```

**Expected result:**

The service should show:

```text
Active: active (running)
```

We can also verify that NGINX is enabled:

```bash
sudo systemctl is-enabled nginx
```

Expected output:

```text
enabled
```

## Step 1.6 – Test NGINX Locally

**What are we doing?**

We are testing the web server from inside the EC2 instance.

**Why are we doing this?**

Before deploying our own website, we should confirm that NGINX is responding correctly.

Run:

```bash
curl -I http://localhost
```

**Expected result:**

We should receive an HTTP response similar to:

```text
HTTP/1.1 200 OK
Server: nginx
```

At this stage, NGINX should serve its default welcome page.

## Step 1.7 – Back Up the Default NGINX Page

Before replacing the default page, create a backup.

Run:

```bash
sudo cp /var/www/html/index.nginx-debian.html /var/www/html/index.nginx-debian.html.backup
```

**Why are we doing this?**

Keeping a backup allows us to retain the original NGINX welcome page for reference or recovery.

## Step 1.8 – Deploy Our Custom HTML Website

**What are we doing?**

We are replacing the default website with our custom multi-cloud dashboard.

**Why are we doing this?**

Using the same HTML website on both cloud providers allows us to compare the hosting environments without changing the application.

Our project contains the website versions in:

```text
website-pages-aws-azure/
```

For this deployment, we use:

```text
aws-website-version-2.html
```

From the local WSL terminal, open a second terminal window and move into the repository directory:

```bash
cd ~/multi-cloud-linux-deployment
```

Copy the HTML file to the EC2 instance:

```bash
scp -i /mnt/d/pem/multicloud-aws-key.pem website-pages-aws-azure/aws-website-version-2.html ubuntu@<AWS_PUBLIC_IP>:/tmp/index.html
```

Replace `<AWS_PUBLIC_IP>` with the EC2 public IPv4 address.

Reconnect to the EC2 instance if necessary:

```bash
ssh -i /mnt/d/pem/multicloud-aws-key.pem ubuntu@<AWS_PUBLIC_IP>
```

Deploy the HTML file into the NGINX web root:

```bash
sudo install -o www-data -g www-data -m 644 /tmp/index.html /var/www/html/index.html
```

Verify the deployed file:

```bash
sudo ls -lh /var/www/html/index.html
```

**Expected result:**

The custom `index.html` file should exist in `/var/www/html/`.

## Step 1.9 – Verify the Custom Website on AWS

First, verify the website locally:

```bash
curl -I http://localhost
```

Expected response:

```text
HTTP/1.1 200 OK
```

Inspect the served HTML:

```bash
curl -s http://localhost | head -20
```

Next, open a web browser on our local computer and enter:

```text
http://<AWS_PUBLIC_IP>/
```

Replace the placeholder with the EC2 public IPv4 address.

**Expected result:**

Our custom multi-cloud dashboard should load in the browser.

**AWS deployment milestone completed:** Ubuntu EC2 provisioned, SSH connectivity verified, NGINX installed, and custom website deployed and publicly accessible.

---

# Phase 2 – Microsoft Azure Linux VM Deployment

## Step 2.1 – Create an Azure Resource Group

**What are we doing?**

We are creating a resource group to organize the Azure resources used in this project.

**Why are we doing this?**

A resource group provides a logical container for related resources, making them easier to manage and clean up.

**How do we do it?**

1. Sign in to the Azure Portal.
2. Search for **Resource groups**.
3. Select **Create**.
4. Choose the subscription.
5. Enter the resource group name:

   `multicloud-linux-rg`

6. Select the region:

   `West US 2`

7. Add the following tags:

| Tag | Value |
|---|---|
| Project | `MultiCloud-Linux-Deployment` |
| Environment | `Learning` |
| ManagedBy | `Manual` |

8. Select **Review + create**, then **Create**.

**Expected result:**

The resource group should be created successfully.

## Step 2.2 – Create an Ubuntu Linux Virtual Machine

**What are we doing?**

We are provisioning an Ubuntu virtual machine in Microsoft Azure.

**Why are we doing this?**

We need a second Linux environment to deploy the same website and compare the cloud platforms.

**How do we do it?**

1. Search for **Virtual machines** in the Azure Portal.
2. Select **Create** → **Azure virtual machine**.
3. Under Project details:
   - Subscription: Select your Azure subscription.
   - Resource group: `multicloud-linux-rg`.
4. Under Instance details:
   - Virtual machine name: `multicloud-azure-vm`.
   - Region: `West US 2`.
   - Security type: Standard.
   - Image: Ubuntu Server 24.04 LTS.
   - Size: `Standard_B2ats_v2`.
5. Under Administrator account:
   - Authentication type: SSH public key.
   - Username: `azureuser`.
   - Generate a new SSH key pair.
   - Key pair name: `multicloud-azure-vm_key`.
   - Download the private key and store it securely.
6. Under Inbound port rules, allow SSH (22) as required for initial setup. We will verify and restrict the NSG rule to our IP address.
7. Under Disks:
   - Select Standard SSD locally redundant storage (LRS) for the OS disk.
8. Under Networking:
   - Use the created virtual network and subnet.
   - Create or select the network security group.
   - Ensure a public IP is configured.
9. Under Management, configure auto-shutdown if desired. In our lab, auto-shutdown was configured for 19:00 IST.
10. Review the configuration and select **Create**.
11. Download and securely store the generated private key.

**Expected result:**

Azure should deploy the virtual machine successfully.

Our lab configuration:

| Setting | Value |
|---|---|
| Resource group | `multicloud-linux-rg` |
| VM name | `multicloud-azure-vm` |
| Region | West US 2 |
| Operating system | Ubuntu 24.04.5 LTS |
| VM size | `Standard_B2ats_v2` |
| vCPU | 1 |
| Reported memory | Approximately 887 MiB |
| OS disk | Standard SSD LRS |
| Username | `azureuser` |
| SSH authentication | SSH key pair |

## Step 2.3 – Configure Azure Network Security Group Rules

**What are we doing?**

We are configuring inbound network rules to control traffic reaching the Azure virtual machine.

**Why are we doing this?**

SSH is required for administration, while HTTP is required for public website access.

**How do we do it?**

1. Open the Azure virtual machine.
2. Select **Networking**.
3. Open the associated Network Security Group (NSG).
4. Navigate to **Inbound security rules**.
5. Add or verify these rules:

| Rule | Priority | Protocol | Port | Source | Action |
|---|---:|---|---:|---|---|
| HTTP | 300 | TCP | 80 | Any | Allow |
| SSH | 320 | TCP | 22 | My IP address (`/32`) | Allow |

6. Save the rules.

**Important:** Restrict SSH to your current public IP address using `/32`. If your public IP changes, update the SSH rule before connecting again. HTTP is publicly accessible for this demonstration.

**Expected result:**

The NSG should allow HTTP traffic on port 80 and SSH traffic on port 22 only from the configured client IP.

## Step 2.4 – Connect to the Azure VM Using SSH

**What are we doing?**

We are connecting to the Azure Ubuntu VM from our local computer.

**How do we do it?**

Open Git Bash on Windows.

If the downloaded private key is stored in `D:\pem`, copy it into the Git Bash SSH directory:

```bash
mkdir -p ~/.ssh
cp /d/pem/multicloud-azure-vm_key.pem ~/.ssh/
chmod 400 ~/.ssh/multicloud-azure-vm_key.pem
```

Connect to the VM:

```bash
ssh -i ~/.ssh/multicloud-azure-vm_key.pem azureuser@<AZURE_PUBLIC_IP>
```

Replace `<AZURE_PUBLIC_IP>` with the public IP shown in the Azure Portal.

**Expected result:**

We should successfully connect to the Azure Ubuntu VM.

Verify the logged-in user:

```bash
whoami
```

Expected output:

```text
azureuser
```

Check the hostname:

```bash
hostname
```

Expected output:

```text
multicloud-azure-vm
```

## Step 2.5 – Verify Azure Linux System Information

Run the following commands:

```bash
cat /etc/os-release
```

```bash
uname -r
```

```bash
nproc
```

```bash
free -h
```

```bash
df -h
```

**Why are we doing this?**

We are inspecting the operating system, kernel, CPU, memory and disk resources before installing the web server.

**Expected result:**

The VM should report Ubuntu 24.04.5 LTS, one CPU, approximately 887 MiB of memory, and its available disk space.

## Step 2.6 – Update Ubuntu Package Information

Run:

```bash
sudo apt update
```

**Expected result:**

Ubuntu should retrieve the latest package information successfully.

## Step 2.7 – Install NGINX on Azure

Run:

```bash
sudo apt install nginx -y
```

Check the service:

```bash
sudo systemctl status nginx --no-pager
```

Expected status:

```text
Active: active (running)
```

Verify that the service is enabled:

```bash
sudo systemctl is-enabled nginx
```

Expected output:

```text
enabled
```

## Step 2.8 – Test NGINX Locally

Run:

```bash
curl -I http://localhost
```

**Expected result:**

The local HTTP request should return:

```text
HTTP/1.1 200 OK
```

At this stage, the default NGINX welcome page should be available.

## Step 2.9 – Back Up the Default NGINX Page

Run:

```bash
sudo cp /var/www/html/index.nginx-debian.html /var/www/html/index.nginx-debian.html.backup
```

This preserves the original NGINX welcome page before we deploy our custom website.

## Step 2.10 – Deploy the Same Custom HTML Website on Azure

**What are we doing?**

We are deploying the same custom dashboard that we hosted on AWS.

**Why are we doing this?**

Using the same application on both platforms gives us a consistent basis for comparing the infrastructure.

Open a local WSL terminal and navigate to the repository:

```bash
cd ~/multi-cloud-linux-deployment
```

Copy the Azure website version to the VM:

```bash
scp -i /mnt/d/pem/multicloud-azure-vm_key.pem website-pages-aws-azure/azure-website-version-2.html azureuser@<AZURE_PUBLIC_IP>:/tmp/index.html
```

Replace `<AZURE_PUBLIC_IP>` with the Azure VM's public IP address.

Connect to the VM if necessary:

```bash
ssh -i /mnt/d/pem/multicloud-azure-vm_key.pem azureuser@<AZURE_PUBLIC_IP>
```

Deploy the file:

```bash
sudo install -o www-data -g www-data -m 644 /tmp/index.html /var/www/html/index.html
```

Verify the file:

```bash
sudo ls -lh /var/www/html/index.html
```

**Expected result:**

The custom HTML file should be present in the NGINX web root.

## Step 2.11 – Verify the Custom Website on Azure

Verify locally:

```bash
curl -I http://localhost
```

Inspect the website content:

```bash
curl -s http://localhost | head -20
```

Next, open a browser and enter:

```text
http://<AZURE_PUBLIC_IP>/
```

**Expected result:**

The custom multi-cloud dashboard should load through the Azure public endpoint.

**Azure deployment milestone completed:** Ubuntu VM provisioned, SSH connectivity verified, NGINX installed, and the same custom website deployed and publicly accessible.

---

# Phase 3 – Multi-Cloud Comparison and Verification

## Step 3.1 – Compare the Virtual Machine Configurations

| Configuration | AWS | Microsoft Azure |
|---|---|---|
| Cloud service | Amazon EC2 | Azure Virtual Machine |
| Region | US West (Oregon) | West US 2 |
| VM name | `multicloud-aws-vm` | `multicloud-azure-vm` |
| Operating system | Ubuntu 26.04.1 LTS | Ubuntu 24.04.5 LTS |
| VM size | `t3.micro` | `Standard_B2ats_v2` |
| vCPU | 2 | 1 |
| Memory | 1 GiB configured | Approximately 887 MiB reported |
| Storage | 8 GiB gp3 EBS | Standard SSD |
| Web server | NGINX | NGINX |

![AWS and Azure VM configuration](comparison-aws-azure/aws-azure-vm-configuration.png)

## Step 3.2 – Compare Network Security

Both virtual machines use inbound network rules to control access.

| Configuration | AWS | Microsoft Azure |
|---|---|---|
| Network security service | Security Group | Network Security Group |
| SSH | TCP 22, restricted to client IP `/32` | TCP 22, restricted to client IP `/32` |
| HTTP | TCP 80, public | TCP 80, public |
| Remote access | SSH key authentication | SSH key authentication |

![AWS and Azure network security comparison](comparison-aws-azure/aws-azure-network-security.png)

## Step 3.3 – Compare Website Deployment

The same custom HTML dashboard was deployed on both Ubuntu virtual machines using NGINX.

![AWS and Azure website deployment comparison](comparison-aws-azure/aws-azure-public-website-deployment.png)

**AWS website:**

![Custom website deployed on AWS](screenshots/aws-screenshots/aws-website.png)

**Azure website:**

![Custom website deployed on Azure](screenshots/az-screenshots/azure-completed-public-website.png)

## Step 3.4 – Understand the Similarities and Differences

### Similarities

- Both platforms provide cloud-based Linux virtual machines.
- Both support SSH key-based authentication.
- Both provide network security services to control inbound traffic.
- Both support NGINX as a web server.
- Both successfully hosted the same custom HTML website.

### Differences

- AWS uses EC2, while Azure provides Virtual Machines.
- AWS uses Security Groups, while Azure uses Network Security Groups.
- VM sizes and resource specifications differ.
- Storage services and configuration options differ.
- Cloud portal layouts and resource management workflows differ.

## Step 3.5 – Common Verification Commands

The following commands were used on both Linux virtual machines.

| Command | Purpose |
|---|---|
| `whoami` | Verify the logged-in user |
| `hostname` | Check the VM hostname |
| `cat /etc/os-release` | Identify the operating system |
| `uname -r` | Check the running kernel |
| `nproc` | Check available CPU cores |
| `free -h` | Inspect memory usage |
| `df -h` | Inspect disk usage |
| `sudo systemctl status nginx --no-pager` | Check NGINX service status |
| `curl -I http://localhost` | Verify the local HTTP response |
| `curl -s http://localhost` | Inspect the served website content |

**Final verification checklist:**

- [x] AWS EC2 instance provisioned.
- [x] AWS SSH connectivity verified.
- [x] AWS Linux system information inspected.
- [x] AWS NGINX installed and running.
- [x] AWS website deployed and publicly accessible.
- [x] Azure Linux VM provisioned.
- [x] Azure SSH connectivity verified.
- [x] Azure Linux system information inspected.
- [x] Azure NGINX installed and running.
- [x] Azure website deployed and publicly accessible.
- [x] Both websites verified using public endpoints.

---

# Phase 4 – Resource Cleanup and Cost Review

**Why is cleanup important?**

Cloud resources may continue to incur charges even when we are no longer using the virtual machines. Stopping a VM does not necessarily remove charges for associated disks, public IP addresses or other retained resources.

Before cleanup, make sure that all required screenshots, website files and project documentation are saved in GitHub.

## Step 4.1 – Clean Up AWS Resources

1. Sign in to the AWS Management Console.
2. Open EC2 → Instances.
3. Select `multicloud-aws-vm`.
4. Choose **Instance state → Terminate instance**.
5. Confirm termination.
6. Open the Security Groups section.
7. Identify the security group created specifically for this lab.
8. Delete it if it is no longer associated with any resources and is safe to remove.
9. Review the EC2 Volumes section to check for unattached EBS volumes.
10. Review Elastic IPs and other resources created for this lab.

**Important:** Terminating the EC2 instance is different from stopping it. Confirm that the correct instance is selected before termination. Do not delete shared or unrelated cloud resources.

The EC2 instance and its root volume should be checked to ensure that the configured delete-on-termination behaviour has removed the volume.

## Step 4.2 – Clean Up Azure Resources

1. Sign in to the Azure Portal.
2. Open **Resource groups**.
3. Select `multicloud-linux-rg`.
4. Review the resources inside the resource group.
5. Confirm that the resources belong to this project and are no longer required.
6. Select **Delete resource group**.
7. Enter the resource group name to confirm deletion.
8. Complete the deletion.

Deleting this resource group removes the project resources contained in it, such as the VM, OS disk, network interface, NSG, public IP and virtual network.

**Important:** Do not delete the resource group until all required project evidence and files have been saved. Check for any resources created outside the resource group separately.

## Step 4.3 – Verify Cleanup

After cleanup:

- Check that the AWS EC2 instance is terminated.
- Check for unattached EBS volumes and unused public IP resources.
- Check that the Azure resource group has been deleted.
- Review both cloud billing or cost-management dashboards.
- Confirm that no unexpected project resources remain.

Record the cleanup date and any resources intentionally retained.

---

# 7. Project Milestones

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
| 09 | Multi-cloud comparison | Completed |
| 10 | Resource cleanup and cost review | Pending |

Update the final milestone after completing and verifying resource cleanup.

# 8. Repository Structure

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

# 9. Screenshots and Documentation

The repository includes screenshots and comparison images captured during the deployment.

- [AWS deployment screenshots](screenshots/aws-screenshots/)
- [Azure deployment screenshots](screenshots/az-screenshots/)
- [AWS and Azure comparison images](comparison-aws-azure/)
- [Website HTML files](website-pages-aws-azure/)
- [Project documentation](docs/)

# 10. Key Learnings

Through this hands-on project, we gained practical experience in:

- Provisioning cloud infrastructure using AWS and Azure.
- Understanding virtual machine configurations and resource sizing.
- Connecting to remote Linux servers using SSH.
- Configuring inbound network security rules.
- Performing Linux system administration.
- Installing and managing NGINX.
- Deploying a custom static website.
- Verifying local and public HTTP connectivity.
- Comparing AWS and Azure infrastructure and networking services.
- Understanding the importance of cloud resource cleanup and cost management.

# 11. Conclusion

In this project, we successfully deployed the same custom HTML website on Ubuntu Linux virtual machines in Amazon Web Services and Microsoft Azure.

We provisioned the infrastructure, configured SSH access and network security, installed NGINX, deployed the website and verified public accessibility on both cloud platforms.

The exercise helped us understand how similar workloads can be hosted across different cloud providers while using their respective compute, networking and storage services.

It also provided a practical foundation in Linux administration, cloud networking, web server deployment and multi-cloud infrastructure comparison.

# Author

**Malathi Shetty**

GitHub: [@withmalathi](https://github.com/withmalathi)

Repository: [multi-cloud-linux-deployment](https://github.com/withmalathi/multi-cloud-linux-deployment)

---

*Building practical cloud skills through hands-on deployment, verification and comparison across AWS and Microsoft Azure.*
