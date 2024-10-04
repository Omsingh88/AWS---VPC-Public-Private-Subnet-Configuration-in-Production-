# AWS VPC Setup for a 2-Tier Application

## Project Description
This example demonstrates how to create a Virtual Private Cloud (VPC) suitable for servers in a production environment. To improve resiliency, you deploy the servers in two Availability Zones using an Auto Scaling group and an Application Load Balancer. For additional security, the servers are deployed in private subnets. The servers receive requests through the load balancer and can connect to the internet via a NAT gateway. To enhance resiliency, the NAT gateway is deployed in both Availability Zones.

## Features
- **2-Tier Architecture**: Separates the web and application tiers for improved organization and security.
- **Auto Scaling Group**: Automatically adjusts the number of EC2 instances based on demand to ensure high availability and fault tolerance.
- **Application Load Balancer**: Distributes incoming traffic across multiple targets, ensuring that the application remains responsive under varying loads.
- **Bastion Host**: Provides secure access to EC2 instances in private subnets.
- **NAT Gateway**: Allows instances in private subnets to access the internet securely.

## Implementation Details
### Step-by-Step Guide

1. **VPC and Resource Creation**:
   - Created a VPC with a CIDR block (e.g., `10.0.0.0/16`) using the "VPC and More" option, which automatically provisions additional resources.
   - The following resources were created:
     - **Public Subnet 1**: For the Application Load Balancer and Bastion Host (e.g., `10.0.1.0/24`).
     - **Private Subnet 1**: For EC2 instances running the application (e.g., `10.0.2.0/24`).
     - **Public Subnet 2**: In the second Availability Zone for redundancy (e.g., `10.0.3.0/24`).
     - **Private Subnet 2**: In the second Availability Zone for redundancy (e.g., `10.0.4.0/24`).

2. **NAT Gateway and Route Table Setup**:
   - An Internet Gateway was attached to the VPC.
   - NAT Gateways were deployed in each public subnet.
   - Custom route tables were created:
     - Public subnets are associated with a route table directing internet-bound traffic to the Internet Gateway.
     - Private subnets are associated with a route table directing traffic to the NAT Gateway for internet access.

3. **Load Balancer and Target Group**:
   - An Application Load Balancer was created to route traffic to EC2 instances running in the private subnets.
   - A target group was set up for the EC2 instances to ensure traffic is directed correctly.

4. **Auto Scaling Group Configuration**:
   - An Auto Scaling group was configured to ensure that a specified number of EC2 instances are running at all times, automatically scaling in or out based on the defined policies.

## Application Access
The application running on the private servers can be accessed using the DNS name of the load balancer, ensuring secure and scalable access to the service.

### Example URL
- The application can be accessed at: `http://<your-load-balancer-dns-name>`

## Screenshots
Below are screenshots demonstrating the different stages of the project setup and execution:

### 1. Creating the VPC and Resources
![Creating VPC](screenshots/creating-vpc.png)

### 2. NAT Gateway Setup
![NAT Gateway Setup](screenshots/nat-gateway-setup.png)

### 3. Load Balancer Configuration
![Load Balancer Configuration](screenshots/load-balancer-configuration.png)

### 4. Load Balancer Target Group
![Load Balancer Target Group](screenshots/load-balancer-target-group.png)

### 5. Application Access via Load Balancer
![Application Access](screenshots/application-access.png)

### 6. Auto Scaling Group Configuration
![Auto Scaling Group Configuration](screenshots/auto-scaling-group-configuration.png)

## Future Enhancements
- **Monitoring and Alerts**: Set up CloudWatch for monitoring instance performance and sending alerts.
- **Security Group Optimization**: Review and optimize security group rules for enhanced security.

## Project Summary
**VPC Implementation for Scalable 2-Tier Architecture:**
- Designed a secure AWS VPC for a 2-tier application featuring custom IP ranges, private subnets, route tables, and security groups.
- Launched the application utilizing an Elastic Load Balancer (ELB) target group and Auto Scaling group, ensuring controlled access and validated connectivity.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
