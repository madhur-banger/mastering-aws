![a w s-v p c](https://github.com/saikiranpi/mastering-aws/assets/109568252/51f3bbf7-0706-450b-abf5-8c4bd697911b)


############## AWS VPC ####################

# AWS VPC Setup Guide

Welcome to the AWS VPC Setup Guide! This guide will walk you through the process of creating a Virtual Private Cloud (VPC) along with its components such as subnets, Internet Gateway, and Routing tables.

## What is VPC?

A Virtual Private Cloud (VPC) is a virtual network environment within AWS that allows you to create a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define.

## Creating VPC

To create a VPC:
1. Go to the AWS Management Console.
2. Navigate to the VPC dashboard.
3. Click on "Create VPC" and specify the VPC details such as CIDR block.

## Creating Subnets & Internet Gateway

### Subnets
Subnets are subdivisions of a VPC's IP address range. They help organize and manage different parts of your network.

Imagine a large plot of land that you want to develop into a residential area. Subnets are like individual buildings within this plot, each containing multiple flats. 

To create subnets:
1. Navigate to the VPC dashboard.
2. Click on "Subnets" and then "Create Subnet".
3. Specify the subnet details including CIDR block and Availability Zone (AZ).

### Internet Gateway (IGW)
An Internet Gateway allows communication between instances in your VPC and the internet.

To create an Internet Gateway:
1. Navigate to the VPC dashboard.
2. Click on "Internet Gateways" and then "Create Internet Gateway".
3. Attach the Internet Gateway to your VPC.

## Creating Routing Tables

Routing tables define how traffic is directed within the VPC. They control the flow of traffic between subnets, internet gateways, and other network devices within the VPC.

To create a routing table:
1. Navigate to the VPC dashboard.
2. Click on "Route Tables" and then "Create Route Table".
3. Define the routing rules, ensuring that traffic flows efficiently and securely to its intended destination.

## Example on VPC

On a high level, each company's data and applications are kept separate and secure within their own VPC. Subnets help organize different stages of the software development lifecycle. 

![VPC Setup Diagram](vpc_setup.png)

Now you have configured VPC and Subnets successfully!

## Internet Gateway & Route Tables

### Internet Gateway (IGW)
An Internet Gateway allows communication between instances in your VPC and the internet.

### Route Tables
Route tables control the flow of traffic within the VPC. They ensure that traffic is directed efficiently and securely to its intended destination.

To configure Internet Gateway and Route Tables:
1. Create an Internet Gateway and attach it to your VPC.
2. Create a Route Table and define routing rules, allowing traffic to flow between subnets and the internet.

Remember to allow public subnets to access the internet by configuring the route table appropriately.

***Note:*** In routing tables, `0.0.0.0/0` means traffic not destined for the local network (e.g., `10.35.0.0/16`) should be routed to the internet gateway. 

Now you have a fully functional VPC with its components set up properly! Happy networking!

![image](https://github.com/saikiranpi/mastering-aws/assets/109568252/97947faf-5b41-41da-9be0-78fd4e495250)
Amazon Web Services (AWS) Virtual Private Cloud (VPC) is one of the foundational services within the AWS cloud, allowing you to create a logically isolated network environment. In a VPC, you can configure various components such as subnets, route tables, and internet gateways to define your network architecture. Let’s break down each of these components in detail, along with their roles and best practices.

---

### **1. Virtual Private Cloud (VPC)**

#### **Overview**
A Virtual Private Cloud (VPC) is a virtual network that allows AWS resources like EC2 instances, RDS databases, and Lambda functions to run in an isolated network. It provides full control over networking, including IP addressing, subnets, route tables, and gateways. You can also establish connectivity between your VPC and the internet, private data centers, or other AWS services.

#### **Components of a VPC:**
- **CIDR Block**: A VPC is defined by its Classless Inter-Domain Routing (CIDR) block, which is the IP address range for the network. For example, a `/16` block allows 65,536 IP addresses. Commonly used private IP address ranges include `10.0.0.0/8`, `172.16.0.0/12`, and `192.168.0.0/16`.
- **Subnets**: A VPC is divided into subnets, each with its own IP range. You can create both **public subnets** (with internet access) and **private subnets** (without direct internet access).
- **Route Tables**: Each subnet in a VPC is associated with a route table that dictates how traffic is routed within and outside the VPC.
- **Internet Gateway**: This allows internet access for resources within your VPC.
- **NAT Gateway or NAT Instances**: These provide internet access to resources in private subnets without exposing them to the internet.

#### **VPC Use Cases:**
- Hosting websites and applications
- Running databases and backend services securely
- Creating hybrid cloud environments with on-premise data centers
- Isolating environments (dev, test, prod) for secure operation

#### **Best Practices for VPC**:
- **Use Multiple Availability Zones**: Distribute subnets across multiple Availability Zones (AZs) for high availability and fault tolerance.
- **Subnet Sizing**: Plan your subnet size carefully to avoid exhausting IP addresses, keeping the future scalability of your application in mind.
- **Tagging**: Use tags to organize and manage your VPC resources effectively.
  
---

### **2. Subnets**

#### **Overview**
A subnet is a sub-division of the VPC’s IP address range. Each subnet is associated with an Availability Zone (AZ). Subnets determine the range of IP addresses available to resources deployed within them.

#### **Types of Subnets**:
- **Public Subnet**: Resources in public subnets can communicate with the internet via an Internet Gateway (IGW). EC2 instances in a public subnet must have a public IP or Elastic IP (EIP) to communicate with the internet.
- **Private Subnet**: Resources in private subnets do not have direct internet access. They can, however, access the internet through a NAT Gateway or NAT Instance, which provides outbound internet access while keeping resources hidden from inbound traffic.

#### **Best Practices for Subnets**:
- **Use Public Subnets for Frontend Services**: Place load balancers and bastion hosts in public subnets, which can communicate with the internet.
- **Use Private Subnets for Backend Services**: Databases, backend servers, and internal services should reside in private subnets for added security.
- **Size Appropriately**: A `/24` subnet gives 256 IPs, with 251 usable IP addresses (after accounting for reserved addresses). Plan the subnet size based on expected resource scaling.
- **Separate Traffic**: Use different subnets for different types of traffic (e.g., database, application, web) to enforce separation and security.

---

### **3. Route Tables**

#### **Overview**
A **route table** defines how traffic is routed between subnets and other destinations. Each subnet must be associated with a route table that contains rules (routes) to specify where network traffic should be directed.

#### **Components of a Route Table**:
- **Destination**: The destination CIDR block (e.g., `0.0.0.0/0` for all external traffic, or another VPC’s CIDR for VPC Peering).
- **Target**: The target is where the traffic will be routed. This could be:
  - An Internet Gateway (IGW) for public subnets.
  - A NAT Gateway for private subnets that need outbound internet access.
  - A Virtual Private Gateway for VPN or Direct Connect connections.
  - Another VPC for traffic routed between peered VPCs.

#### **Best Practices for Route Tables**:
- **Use Separate Route Tables for Public and Private Subnets**: Public subnets route traffic destined for the internet through the Internet Gateway. Private subnets should route through a NAT Gateway for outbound internet traffic.
- **Avoid Circular Routes**: When setting up VPC peering or VPN connections, ensure that routes don’t create a loop.
- **Enable Propagation for VPNs**: When connecting to an on-premises network via VPN or Direct Connect, enable route propagation for dynamic routing.

#### **Hands-On Example**:
In a public subnet, your route table might look like this:
- **Destination**: `0.0.0.0/0` (all traffic outside the VPC)
- **Target**: `igw-xxxxxxxx` (the Internet Gateway)

In a private subnet, the route table might look like this:
- **Destination**: `0.0.0.0/0` (all internet-bound traffic)
- **Target**: `nat-xxxxxxxx` (the NAT Gateway)

---

### **4. Internet Gateway (IGW)**

#### **Overview**
An **Internet Gateway (IGW)** is a horizontally scaled, redundant, and highly available component that allows communication between instances in a VPC and the internet. It enables your VPC to send and receive traffic to and from the public internet.

#### **Key Points**:
- Each VPC can have only one Internet Gateway.
- The IGW is responsible for providing access to resources like EC2 instances in public subnets to the internet.
- Only instances with public IP addresses (or Elastic IPs) can use the IGW to send and receive traffic.

#### **Best Practices for Internet Gateways**:
- **Assign Elastic IPs**: Attach Elastic IPs to instances in public subnets to ensure they have persistent public-facing IP addresses.
- **Secure Public Subnets**: Instances in public subnets should be protected with proper security groups and Network Access Control Lists (NACLs).
- **Monitor Traffic**: Use AWS CloudTrail to monitor API calls related to the IGW. Implement logging via VPC Flow Logs to track all traffic through the IGW.

#### **Hands-On Example**:
1. **Create an Internet Gateway**: 
   - Navigate to the VPC dashboard, click on "Internet Gateways", and create a new IGW.
   - Attach the IGW to your VPC.
2. **Update Route Table**:
   - Update the route table associated with your public subnet to direct traffic (`0.0.0.0/0`) to the IGW.
3. **Assign Public IPs**:
   - Ensure instances in the public subnet have a public or Elastic IP for internet connectivity.

---

### **5. NAT Gateway and NAT Instance**

#### **Overview**
While an Internet Gateway allows inbound and outbound traffic for public subnets, resources in **private subnets** don’t have direct access to the internet. To provide **outbound internet access** (e.g., for downloading software updates) to resources in private subnets, you need a **NAT Gateway** or **NAT Instance**.

- **NAT Gateway**: A managed service that scales automatically and is fault-tolerant.
- **NAT Instance**: A user-managed EC2 instance configured to provide NAT capabilities (less recommended due to management overhead).

#### **Best Practices for NAT Gateways**:
- **Use NAT Gateways in Multiple AZs**: This ensures high availability.
- **Place in Public Subnet**: NAT Gateways must be in a public subnet to provide internet access to instances in private subnets.
- **Monitor Usage**: Use CloudWatch to monitor NAT Gateway usage and avoid bottlenecks.

#### **Hands-On Example**:
1. **Create a NAT Gateway**:
   - Place the NAT Gateway in a public subnet with an Elastic IP attached.
2. **Update Private Subnet Route Table**:
   - Update the route table for private subnets to route outbound traffic (`0.0.0.0/0`) through the NAT Gateway.

---

### **Conclusion and Best Practices for a VPC Setup**

- **Multi-AZ Setup**: Always deploy your VPC components across multiple Availability Zones for redundancy and high availability.
- **Security**: Implement proper security using Security Groups and Network ACLs to control traffic in and out of your VPC.
- **Monitoring and Logging**: Use VPC Flow Logs to capture traffic information and AWS CloudTrail to monitor API calls.
- **Use Cases**: VPC is ideal for web hosting, multi-tier applications, big data processing, secure backend systems, and hybrid cloud architectures.

This detailed analysis covers the key aspects of AWS VPC, subnets, route tables, and internet gateways. By mastering these components, you’ll be well on your way to becoming a proficient cloud engineer!

# Demo Tutorial for all I studied today- 
Here’s a step-by-step guide to create a Virtual Private Cloud (VPC) with both public and private subnets, configure route tables, set up a NAT gateway for the private subnet, and create an EC2 instance in the private subnet that can access the internet. Finally, you'll ping `google.com` to verify connectivity.

### Step 1: Create a VPC

1. **Log in to AWS Management Console**.
2. **Navigate to VPC**:
   - Go to the **Services** menu and select **VPC**.

3. **Create a VPC**:
   - Click on **Your VPCs** in the left panel.
   - Click **Create VPC**.
   - Configure the VPC settings:
     - **Name tag**: `MyVPC`
     - **IPv4 CIDR block**: `10.0.0.0/16`
     - **Tenancy**: Default
   - Click **Create VPC**.

### Step 2: Create Subnets

#### 2.1 Create a Public Subnet

1. Click on **Subnets** in the left panel.
2. Click **Create Subnet**.
3. Configure the public subnet settings:
   - **Name tag**: `PublicSubnet`
   - **VPC**: `MyVPC`
   - **Availability Zone**: Select any available AZ (e.g., `us-east-1a`).
   - **IPv4 CIDR block**: `10.0.1.0/24`
4. Click **Create Subnet**.

#### 2.2 Create a Private Subnet

1. Click **Create Subnet** again.
2. Configure the private subnet settings:
   - **Name tag**: `PrivateSubnet`
   - **VPC**: `MyVPC`
   - **Availability Zone**: Select the same AZ as above.
   - **IPv4 CIDR block**: `10.0.2.0/24`
3. Click **Create Subnet**.

### Step 3: Create an Internet Gateway

1. Click on **Internet Gateways** in the left panel.
2. Click **Create Internet Gateway**.
3. Configure the Internet Gateway settings:
   - **Name tag**: `MyInternetGateway`
4. Click **Create Internet Gateway**.
5. Attach the Internet Gateway to your VPC:
   - Select the newly created Internet Gateway.
   - Click **Actions** > **Attach to VPC**.
   - Select `MyVPC` and click **Attach Internet Gateway**.

### Step 4: Create Route Tables

#### 4.1 Create a Public Route Table

1. Click on **Route Tables** in the left panel.
2. Click **Create Route Table**.
3. Configure the public route table settings:
   - **Name tag**: `PublicRouteTable`
   - **VPC**: `MyVPC`
4. Click **Create Route Table**.
5. Select the new route table and click **Edit routes**.
6. Click **Add route**:
   - **Destination**: `0.0.0.0/0`
   - **Target**: Select your Internet Gateway (`MyInternetGateway`).
7. Click **Save routes**.

8. **Associate the Public Route Table**:
   - Go to **Subnet Associations**.
   - Click **Edit subnet associations**.
   - Select the public subnet (`PublicSubnet`) and click **Save**.

#### 4.2 Create a Private Route Table

1. Click **Create Route Table** again.
2. Configure the private route table settings:
   - **Name tag**: `PrivateRouteTable`
   - **VPC**: `MyVPC`
3. Click **Create Route Table**.
4. **Associate the Private Route Table**:
   - Go to **Subnet Associations**.
   - Click **Edit subnet associations**.
   - Select the private subnet (`PrivateSubnet`) and click **Save**.

### Step 5: Create a NAT Gateway

1. **Allocate an Elastic IP Address**:
   - Go to **Elastic IPs** in the VPC dashboard.
   - Click **Allocate Elastic IP address** and then click **Allocate**.

2. **Create NAT Gateway**:
   - Go to **NAT Gateways** in the left panel.
   - Click **Create NAT Gateway**.
   - Configure the NAT Gateway settings:
     - **Name tag**: `MyNATGateway`
     - **Subnet**: Select `PublicSubnet`.
     - **Elastic IP Allocation ID**: Select the Elastic IP you just allocated.
   - Click **Create NAT Gateway**.

3. **Update the Private Route Table**:
   - Select the `PrivateRouteTable`.
   - Click **Edit routes** and add a new route:
     - **Destination**: `0.0.0.0/0`
     - **Target**: Select your NAT Gateway (`MyNATGateway`).
   - Click **Save routes**.

### Step 6: Create an EC2 Instance in the Private Subnet

1. Go to the **EC2 Dashboard**.
2. Click **Launch Instance**.
3. Configure the instance settings:
   - **Name**: `MyPrivateInstance`
   - **AMI**: Select an Amazon Linux 2 or Ubuntu AMI.
   - **Instance Type**: Select an instance type (e.g., `t2.micro`).
   - **Network Settings**:
     - **VPC**: Select `MyVPC`.
     - **Subnet**: Select `PrivateSubnet`.
     - **Auto-assign Public IP**: Disable.
   - Configure the rest of the settings as needed (storage, tags, security groups).
4. Click **Launch** and select an existing key pair or create a new one.
5. Click **Launch Instances**.

### Step 7: Connect to the EC2 Instance

Since the instance is in a private subnet, you will need to connect to it using an SSH client through a bastion host or via the public instance.

1. **Launch a Bastion Host** (EC2 in the public subnet):
   - Follow the same steps to launch an EC2 instance in the `PublicSubnet`.
   - Ensure you can SSH into this instance.
   
2. **SSH into the Bastion Host**:
   ```bash
   ssh -i your-key.pem ec2-user@<Public-Instance-IP>
   ```

3. **From the Bastion Host, SSH into the Private Instance**:
   ```bash
   ssh -i your-key.pem ec2-user@<Private-Instance-IP>
   ```

### Step 8: Ping google.com

1. Once you are connected to the private EC2 instance, run the following command:
   ```bash
   ping google.com
   ```

2. **Check the output**. If everything is set up correctly, you should see responses from `google.com`, indicating successful internet access from the private subnet through the NAT gateway.

### Summary

You’ve successfully created a VPC with public and private subnets, set up route tables and a NAT gateway, and launched an EC2 instance in the private subnet that can access the internet. This setup is a common architecture used in AWS for secure application deployments. If you have any questions or need further assistance, feel free to ask!
