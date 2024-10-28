# AWS - VPC Public-Private Subnet Configuration in Production 

## Project Description
This example demonstrates how to create a Virtual Private Cloud (VPC) suitable for servers in a production environment. To improve resiliency, you deploy the servers in two Availability Zones using an Auto Scaling group and an Application Load Balancer. For additional security, the servers are deployed in private subnets. The servers receive requests through the load balancer and can connect to the internet via a NAT gateway. To enhance resiliency, the NAT gateway is deployed in both Availability Zones.

## Features
- **2-Tier Architecture**: Separates the web and application tiers for improved organization and security.
- **Auto Scaling Group**: Automatically adjusts the number of EC2 instances based on demand to ensure high availability and fault tolerance.
- **Application Load Balancer**: Distributes incoming traffic across multiple targets, ensuring that the application remains responsive under varying loads.
- **Bastion Host**: Provides secure access to EC2 instances in private subnets.

## Architecture Diagram
![Architecture Diagram](screenshots/architecture-diagram.png)

## Implementation Details
### Step-by-Step Guide

 1. **VPC and Resource Creation**:
   - First, I created a Virtual Private Cloud (VPC) with two public subnets and two private subnets. Each subnet was attached to different route tables and internet gateways to ensure 
     proper network segmentation and security.
     
2. **Auto Scaling Group Configuration**:
   - An Auto Scaling group was configured to ensure that a specified number of EC2 instances are running at all times, automatically scaling in or out based on the defined policies.
     
3. **Creation a Bastion Host**.
   - For secure access to the private subnets, I set up a bastion host. This instance serves as a jump box, allowing me to SSH into the private instances securely. By connecting to the 
     bastion host, I can then access the instances in the private subnets.

4. **Load Balancer and Target Group**:
   - An Application Load Balancer was created to route traffic to EC2 instances running in the private subnets.
   - A target group was set up for the EC2 instances to ensure traffic is directed correctly.


## Application Access
The application running on the private servers can be accessed using the DNS name of the load balancer, ensuring secure and scalable access to the service.

### Example URL
- The application can be accessed at: `http://<your-load-balancer-dns-name>`

## Application Status on EC2 Instances
The application running on the EC2 instances can be verified for its status, ensuring that it is up and running as intended.

## Screenshots
Below are screenshots demonstrating the different stages of the project setup and execution:

### 1. Creating the VPC and Resources
![Creating VPC 1](screenshots/vpc-creation1.png)
![Creating VPC 2](screenshots/vpc-creation2.png)
![Creating VPC 3](screenshots/vpc-creation3.png)

### 2. Auto Scaling Group Configuration
![Auto Scaling Group Configuration 1](screenshots/auto-scaling-group-configuration1.png)
![Auto Scaling Group Configuration 2](screenshots/auto-scaling-group-configuration2.png)
![Auto Scaling Group Configuration 3](screenshots/auto-scaling-group-configuration3.png)
![Auto Scaling Group Configuration 4](screenshots/auto-scaling-group-configuration4.png)
![Auto Scaling Group Configuration 5](screenshots/auto-scaling-group-configuration5.png)

### 3. EC2 Instances Created by Auto Scaling
![EC2 Instances](screenshots/ec2-instances.png)

### 4. Bastion Host Connection to Private EC2
**For secure access to the private subnets, I set up a bastion host. This instance serves as a jump box, allowing me to SSH into the private instances securely. By connecting to the bastion host, I can then access the instances in the private subnets**.

### From my local machine, I used the following SSH command to connect to the bastion host:
```bash
scp -i <path-to-your-pem-file>/your-key.pem ubuntu@<bastion-instance-ip>:/home/ubuntu
```
![Bastion Host Connection](screenshots/bastion-host-connection.png)
![Bastion Host Connection](screenshots/bastion-host-connection1.png)

### 5. Load Balancer and Target Group Creation
![Load Balancer Configuration 1](screenshots/load-balancer-configuration1.png)
![Load Balancer Configuration 2](screenshots/load-balancer-configuration2.png)
![Load Balancer Configuration 4](screenshots/load-balancer-configuration4.png)
![Load Balancer Configuration 5](screenshots/load-balancer-configuration5.png)
![Load Balancer Configuration 6](screenshots/load-balancer-configuration6.png)
![Load Balancer Configuration 7](screenshots/load-balancer-configuration7.png)
![Load Balancer Configuration 8](screenshots/load-balancer-configuration8.png)
![Load Balancer Configuration 9](screenshots/load-balancer-configuration9.png)

### 6. Launching the Web Application
**Finally, I used Python’s built-in HTTP server to host the website. By running the following command, my website became externally visible**:
![Application Status](screenshots/application-status.png)

### 7. Application Access via DNS URL
![Accessing Application 1](screenshots/application-access1.png)
![Accessing Application 2](screenshots/application-access2.png)

