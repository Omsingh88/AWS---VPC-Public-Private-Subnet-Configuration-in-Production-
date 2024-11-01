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

### From my local machine, I used the following SCP command to send the private keys (.pem file) to the bastion host before doing ssh to the instance in priavte subnet:
```bash
scp -i <path-to-your-pem-file>/your-key.pem ubuntu@<bastion-instance-ip>:/home/ubuntu
```
### The above command will copy the PEM file from your computer to the Bastion host. Once the file is successfully copied, move on to the next step. d. SSH into the Bastion host using the following command:
```bash
ssh -i aws-vpc-project.pem ubuntu@10.0.144.22
```
### After SSHing into the Bastion host, use the ls command to check if the aws-vpc-project.pem file is present. If it's not there, double-check your previous commands. f. Now, you can SSH into the private instance using the following command, replacing <private IP> with the private instance's IP address:
```bash
ssh -i aws-vpc-project.pem ubuntu@<private-ip>
```

![Bastion Host Connection](screenshots/bastion-host-connection.png)
![Bastion Host Connection](screenshots/bastion-host-connection1.png)

### 5. Deploying and Launching the Web Application
**We will deploy our application on one of the private instances to test the load balancer. h. After successfully SSHing into the private instance, create an HTML file using the Vim text editor**:
```bash
vim demo.html
```
 **This will open the Vim editor. Copy and paste any HTML content you like into the editor**.

**Finally, I used Python’s built-in HTTP server to host the website. By running the following command, my website became externally visible**:
```python
python3 -m http.server 8000
```
Now, your application is deployed on the private instance on port 8000.


### Note :
We intentionally deployed the application on only one instance to check if the Load Balancer will distribute 50% of the traffic to one instance (which will receive a response) and 50% to another instance (which will not receive a response).
![Application Status](screenshots/application-status.png)

### 6. Load Balancer and Target Group Creation
![Load Balancer Configuration 1](screenshots/load-balancer-configuration1.png)
![Load Balancer Configuration 2](screenshots/load-balancer-configuration2.png)
![Load Balancer Configuration 4](screenshots/load-balancer-configuration4.png)
![Load Balancer Configuration 5](screenshots/load-balancer-configuration5.png)
![Load Balancer Configuration 6](screenshots/load-balancer-configuration6.png)
![Load Balancer Configuration 7](screenshots/load-balancer-configuration7.png)
![Load Balancer Configuration 8](screenshots/load-balancer-configuration8.png)
![Load Balancer Configuration 9](screenshots/load-balancer-configuration9.png)



### 7. Application Access via DNS URL
![Accessing Application 1](screenshots/application-access1.png)
![Accessing Application 2](screenshots/application-access2.png)

