# AWS - VPC Public-Private Subnet Configuration in Production 

## Project Description
This project's overview is depicted in the diagram below. The setup revolves around a Virtual Private Cloud (VPC) featuring both public and private subnets, thoughtfully distributed across two Availability Zones to ensure reliability.

Within each public subnet, there's a NAT gateway to facilitate outbound internet connectivity and a load balancer node for effective traffic distribution.

On the other hand, the project's servers reside in the private subnets. Their deployment and termination are automated through an Auto Scaling group, allowing them to dynamically adapt to workload changes. These servers play a pivotal role in receiving traffic from the load balancer and can access the internet through the NAT gateway when necessary

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
```python
python3 -m http.server 8000
```
![Application Status](screenshots/application-status.png)

### 7. Application Access via DNS URL
![Accessing Application 1](screenshots/application-access1.png)
![Accessing Application 2](screenshots/application-access2.png)

