# AWS VPC Setup with Public and Private Subnets

This guide walks you through setting up an Amazon Web Services (AWS) Virtual Private Cloud (VPC) with both public and private subnets.

---

## Step 1: Create a VPC

1. **Log in to the AWS Management Console**.
2. **Navigate to the VPC Dashboard**.
3. **Create a VPC**:
   - Click on "Your VPCs" in the left-hand menu.
   - Click the "Create VPC" button.
   - Enter a name for your VPC.
   - Specify an IPv4 CIDR block (e.g., `10.0.0.0/16`).
   - Click "Create".

---

## Step 2: Create Subnets

### Create a Public Subnet
1. In the VPC Dashboard, click on "Subnets" in the left-hand menu.
2. Click the "Create subnet" button.
3. Select the VPC you just created.
4. Enter a name for the subnet (e.g., `PublicSubnet`).
5. Specify an IPv4 CIDR block for the subnet (e.g., `10.0.1.0/24`).
6. Choose an Availability Zone.
7. Click "Create".

### Create a Private Subnet
1. Repeat the process to create a private subnet.
2. Enter a name for the subnet (e.g., `PrivateSubnet`).
3. Specify an IPv4 CIDR block for the subnet (e.g., `10.0.2.0/24`).
4. Choose a different Availability Zone if desired.
5. Click "Create".

---

## Step 3: Create an Internet Gateway

1. **Navigate to the Internet Gateways Section**:
   - In the VPC Dashboard, click on "Internet Gateways" in the left-hand menu.
   - Click the "Create internet gateway" button.
   - Enter a name for the internet gateway.
   - Click "Create".

2. **Attach the Internet Gateway to Your VPC**:
   - Select the internet gateway you just created.
   - Click the "Actions" button and select "Attach to VPC".
   - Select your VPC from the list.
   - Click "Attach".

---

## Step 4: Create a Route Table for the Public Subnet

1. **Navigate to the Route Tables Section**:
   - In the VPC Dashboard, click on "Route Tables" in the left-hand menu.
   - Click the "Create route table" button.
   - Enter a name for the route table (e.g., `PublicRouteTable`).
   - Select your VPC.
   - Click "Create".

2. **Associate the Public Subnet with the Route Table**:
   - Select the route table you just created.
   - Click the "Subnet Associations" tab.
   - Click "Edit subnet associations".
   - Select the public subnet.
   - Click "Save".

3. **Add a Route to the Internet Gateway**:
   - Select the route table you just created.
   - Click the "Routes" tab.
   - Click "Edit routes".
   - Click "Add route".
   - Set the destination to `0.0.0.0/0`.
   - Set the target to the internet gateway you created.
   - Click "Save routes".

---

## Step 5: Create a NAT Gateway for the Private Subnet

1. **Allocate an Elastic IP Address**:
   - In the VPC Dashboard, click on "Elastic IPs" in the left-hand menu.
   - Click the "Allocate Elastic IP address" button.
   - Click "Allocate".

2. **Create a NAT Gateway**:
   - In the VPC Dashboard, click on "NAT Gateways" in the left-hand menu.
   - Click the "Create NAT Gateway" button.
   - Select the public subnet.
   - Select the Elastic IP you just allocated.
   - Click "Create NAT Gateway".

---

## Step 6: Create a Route Table for the Private Subnet

1. **Create a Route Table**:
   - In the VPC Dashboard, click on "Route Tables" in the left-hand menu.
   - Click the "Create route table" button.
   - Enter a name for the route table (e.g., `PrivateRouteTable`).
   - Select your VPC.
   - Click "Create".

2. **Associate the Private Subnet with the Route Table**:
   - Select the route table you just created.
   - Click the "Subnet Associations" tab.
   - Click "Edit subnet associations".
   - Select the private subnet.
   - Click "Save".

3. **Add a Route to the NAT Gateway**:
   - Select the route table you just created.
   - Click the "Routes" tab.
   - Click "Edit routes".
   - Click "Add route".
   - Set the destination to `0.0.0.0/0`.
   - Set the target to the NAT Gateway you created.
   - Click "Save routes".

---

## Step 7: Configure Security Groups and Network ACLs

1. **Create Security Groups**:
   - In the VPC Dashboard, click on "Security Groups" in the left-hand menu.
   - Create security groups for your instances in the public and private subnets, allowing necessary inbound and outbound traffic.

2. **Configure Network ACLs**:
   - In the VPC Dashboard, click on "Network ACLs" in the left-hand menu.
   - Create or modify Network ACLs to control traffic at the subnet level.

---

## Step 8: Launch Instances

1. **Launch an Instance in the Public Subnet**:
   - Navigate to the EC2 Dashboard.
   - Launch an instance in the public subnet, associating it with the public subnet and the appropriate security group.

2. **Launch an Instance in the Private Subnet**:
   - Launch another instance in the private subnet, associating it with the private subnet and the appropriate security group.

---

## Step 9: Test the Setup

1. **Test Connectivity**:
   - SSH into the instance in the public subnet.
   - Verify that it can access the internet.
   - Verify that the instance in the private subnet can access the internet through the NAT Gateway.

2. **Test Security**:
   - Ensure that the instance in the private subnet cannot be directly accessed from the internet.

---

## Conclusion

You have now set up a VPC with both public and private subnets. The public subnet allows instances to communicate with the internet, while the private subnet keeps instances secure and only allows outbound internet access through the NAT Gateway. This setup is ideal for hosting web applications where you need to expose certain components to the internet while keeping others secure.
