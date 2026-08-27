# ☁️ AWS Cloud Infrastructure: Custom VPC & Secure Web Server Deployment

**AWS cloud infrastructure project demonstrating foundational networking, security, and EC2 instance lifecycle management.**

## Project Overview
This project demonstrates the manual construction of a foundational AWS networking environment and the deployment of a scalable compute instance. Rather than relying on default AWS configurations, I built a custom Virtual Private Cloud (VPC) from the ground up to ensure complete control over routing, security, and resource isolation, ultimately hosting a static Apache web server.

## 🏗️ Architecture & Core Components
* **Virtual Private Cloud (VPC):** Configured a custom VPC (`10.0.0.0/16`) to establish an isolated network boundary.
* **Public Subnet & IGW:** Provisioned a public subnet (`10.0.1.0/24`) and attached an Internet Gateway to enable external routing.
* **Custom Routing:** Created a custom Route Table directing outbound internet traffic (`0.0.0.0/0`) to the IGW.
* **Defense-in-Depth Security:** 
    * Implemented a **Network ACL** (stateless) for subnet-level perimeter control.
    * Configured a **Security Group** (stateful) to allow inbound HTTP (Port 80) and restricted SSH (Port 22) access at the instance level.
* **Elastic Compute Cloud (EC2):** Launched an Amazon Linux 2023 instance deployed directly into the custom public subnet.

## 🚀 Deployment Steps

**1. Network Foundation**
Created the VPC, Subnet, and Internet Gateway, ensuring auto-assign public IPv4 was enabled for the subnet.

**2. Automated Web Server Provisioning**
Passed the following script into the EC2 User Data to automatically install, start, and configure the Apache web server on boot:

    #!/bin/bash
    yum -y install httpd
    systemctl enable httpd
    systemctl start httpd
    echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html

**3. Infrastructure Scaling & Lifecycle Management**
* **Termination Protection:** Enabled safeguards against accidental instance deletion.
* **Vertical Scaling:** Stopped the instance to upgrade the instance type from `t3.micro` to `t3.small` to double memory capacity.
* **Storage Expansion:** Dynamically expanded the root EBS volume from 8 GiB to 10 GiB.
