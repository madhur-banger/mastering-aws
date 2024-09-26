![vpc PEERING](https://github.com/saikiranpi/mastering-aws/assets/109568252/982bf754-b276-4154-8e4a-9c4b1f1294f0)


### **VPC Peering Guide**

This guide provides a comprehensive step-by-step approach to setting up VPC peering between different Virtual Private Clouds (VPCs) in AWS, especially when the VPCs are in different regions or availability zones.

### **Scenario Overview**

**Real-World Example:**
Imagine you work for a multinational corporation (MNC) with critical data hosted across AWS regions, specifically in the US East (Ohio) and EU (Ireland). Your company needs seamless, secure, and low-latency communication between these geographically dispersed VPCs.

#### **Before VPC Peering**
Without VPC peering, resources in separate VPCs (especially those in different regions) cannot communicate directly. This scenario often leads to:
- Increased latency as traffic may have to go through the public internet.
- Higher security risks and exposure to the internet.
- Potential extra costs due to data transfer fees via public endpoints.
- Challenges in managing secure, reliable communication between VPCs.

#### **After VPC Peering**
By setting up VPC peering:
- You create a private and direct connection between VPCs, significantly enhancing security and performance.
- Reduced latency as data flows directly through AWS’s private network.
- No exposure of traffic to the internet, which reduces security threats.
- Cost-effective data transfer between VPCs compared to other connection types like VPN or Direct Connect.

### **Detailed Step-by-Step VPC Peering Setup**

#### **1. Visualize the Architecture**
Before starting, draw a network diagram of the VPCs and how you intend to peer them. This helps in understanding the network topology, CIDR ranges, and ensuring there are no overlapping IP ranges. Here's a basic example:

- **VPC 1 (US East 1A)**: 10.1.0.0/16
- **VPC 2 (US East 1A)**: 172.16.0.0/16
- **VPC 3 (US East 2A)**: 192.168.0.0/16

These CIDR ranges should be unique across all VPCs to avoid conflicts.

#### **2. Create VPCs**
1. Go to the AWS Management Console.
2. Navigate to **VPC Dashboard** > **Create VPC**.
3. Create three VPCs with the following CIDR blocks:
   - **VPC 1 (US East 1A)**: 10.1.0.0/16
   - **VPC 2 (US East 1A)**: 172.16.0.0/16
   - **VPC 3 (US East 2A)**: 192.168.0.0/16

Ensure that these VPCs do not have overlapping IP address ranges.

#### **3. Launch EC2 Instances in Each VPC**
1. In each VPC, create a subnet and launch an EC2 instance.
2. When launching EC2 instances, include a script in the user data to install Nginx for testing connectivity between the instances. Example user data script:
   ```bash
   #!/bin/bash
   yum update -y
   yum install nginx -y
   systemctl start nginx
   systemctl enable nginx
   ```
3. Ensure that each instance is deployed in a subnet within its respective VPC.

#### **4. Configure Security Groups**
1. **Create Security Groups**: Ensure each EC2 instance has a security group that allows inbound traffic from the other VPC's IP range.
2. **Example Rule for Each Security Group**:
   - **Type**: Custom TCP Rule
   - **Protocol**: TCP
   - **Port Range**: 80 (or other necessary ports)
   - **Source**: IP range of the other VPC (e.g., 10.1.0.0/16 for the first VPC).

#### **5. Set Up VPC Peering Connections**
1. Go to the **VPC Dashboard** > **Peering Connections** > **Create Peering Connection**.
2. **Create the First Peering Connection**:
   - **Requester VPC**: VPC 1 (10.1.0.0/16)
   - **Accepter VPC**: VPC 2 (172.16.0.0/16)
3. **Create the Second Peering Connection**:
   - **Requester VPC**: VPC 1 (10.1.0.0/16)
   - **Accepter VPC**: VPC 3 (192.168.0.0/16)
4. Accept the peering connections:
   - Go to the **Peering Connections** page, select the newly created peering connections, and click **Accept Request**.

#### **6. Update Route Tables**
1. Navigate to **Route Tables** in the VPC Dashboard.
2. Update the route tables for each VPC to include the new peering connections:
   - For **VPC 1**:
     - **Destination**: 172.16.0.0/16, **Target**: Peering Connection to VPC 2
     - **Destination**: 192.168.0.0/16, **Target**: Peering Connection to VPC 3
   - For **VPC 2**:
     - **Destination**: 10.1.0.0/16, **Target**: Peering Connection to VPC 1
   - For **VPC 3**:
     - **Destination**: 10.1.0.0/16, **Target**: Peering Connection to VPC 1

#### **7. Test the Connectivity**
- SSH into the EC2 instances and use `curl` or `ping` commands to test connectivity between the instances in peered VPCs.
- For example, from the EC2 instance in VPC 1, you can `curl` the private IP of the instance in VPC 2 to ensure connectivity.

### **VPC Peering Diagram**
You can create a VPC Peering Diagram showing the network setup, VPCs, instances, and the established peering connections. This helps visualize the connections and ensures the setup is correct.

### **Important Considerations**
- **No IP Overlap**: Ensure that VPCs do not have overlapping CIDR blocks to avoid routing conflicts.
- **No Transitive Peering**: VPC peering does not support transitive routing. A peering connection between VPC A and B, and VPC B and C, does not allow VPC A to communicate with VPC C automatically.

### **Conclusion**
Setting up VPC peering allows seamless and secure communication between VPCs, which can span multiple AWS accounts or regions, improving your network architecture’s efficiency and security.

Happy networking! If you have any further questions or need assistance, feel free to ask!

![image](https://github.com/saikiranpi/mastering-aws/assets/109568252/59795a41-5139-4fed-b43c-793040df240a)
