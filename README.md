**Title:** \
**Name:** Robert Mathisen\
**Date:** 3/11/2021 \
\
**Summary:** \
<br/>
**Reason & Reflection:** \
I think it's important to know how to properly set up a VPC, along with the Public & Private Subnets. Looking at multiple resources, I noticed that some resources tell you to associate the Public & Private Subnets to a Route Table, whereas other resources say to only associate the Private Subnet to the Route Table. This led me down a rabbit hole of trying to figure out which method is correct. While doing this research, I've learned more about Networking (subnet mask; network id; host id; CIDR) and reinforcing the concepts that I already understand through different instructors. 
<br/>
\
\
**Process:** <br/>
\
**1) Create the VPC.** <br/>
VPC → Virtual Private Cloud → Your VPCs \
Notice that there is a VPC that's already been created. This is the Default VPC for this Region! \
Rename this VPC to 'Default_VPC'

VPC → Virtual Private Cloud → Your VPCs → Create VPC \
IPv4 CIDR block: 10.0.0.0/16 (Choose a CIDR block with caution!)...Lets use 10.16.0.0/16 \
Tenancy: Default \
\
Keep in mind that Networks with the same IP address ranges cannot communicate with each other. When picking an IP address range, you need to think about other ranges that you'll potentially be communicating with (ranges used in AWS, for on premises networks, for partners, for venders, for customers, etc). Don't just randomly pick IP ranges, or choose default ranges. It may cause difficulties in the future. Talk to the IT staff of the business! Thoroughly understand which ranges out AWS Network design cannot use. We cannot have overlapping networks. \
\
10.0.y.z - this is probably one of the most common address ranges because it's used as a default \
10.1.y.z - this is probably the 2nd most commonly used ranges, as it's a way to avoid the default of 10.0.y.z \
Avoid ranges all the way up to 10.10.y.z just to be safe \
10.16.0.0 - this might be a good starting point as it will most likely be not common to use. But, once again, check with the IT staff!
<br/><br/><br/>
**2) Create the Public & Private Subnets.** <br/><br/>
The Network ID for 10.16.0.0/16 is 10.16.y.z with a Subnet Mask of 255.255.0.0 (65,536 IP addresses). \
We can Subnet this which means to divide Network ID into multiple networks. \
If we wanted to, we can create multiple /24 subnets ranging from 10.16.0.z to 10.16.255.z \
Your Network IDs will be 10.16.0.0, 10.16.1.z, 10.16.2.z, ... , 10.16.255.z 

If we wanted to subnet the 10.16.0.0/16 to /24,
we would have the first (2) octects 10.16 being static, then have the freedom to change the next 8 binary values which would result in being able to create 256 Subnets (2^8). <br/> Each /28 subnet has 16 IP addresses. \
256 IP addresses per /24 Subnet x 256 Individual Subnets = 65,536 IP addresses! \
IPv4 Addresses that you assign cannot end with a 0 or 255 (these are reserved), but Subnets can absolutely have these values.

If we wanted to, we could equally take the 10.16.0.0/16 to /28 \
We would have the first (2) octects 10.16 being static, then have the freedom to change the next 12 binary values which would result in being able to create 4096 Subnets (2^12). <br/> Each /28 subnet has 16 IP addresses. \
16 IP addresses per /28 Subnet x 4096 Individual Subnets = 65,536 IP addresses
<br/>
<br/>
<br/>
VPC → Virtual Private Cloud → Subnets → Create Subnet\
VPC ID: select the VPC created in Step 1 \
Subnet Name: **Public**Subnet \
Availability Zone: us-east-1**a** \
IPv4 CIDR block: 10.16.0.**1**/24

VPC ID: select the VPC created in Step 1 \
Subnet Name: **Private**Subnet \
Availability Zone: us-east-1**b** \
IPv4 CIDR block: 10.16.0.**2**/24
\
\
**3) Create the Internet Gateway.** - allows you to connect the Public Subnet to the Internet! <br/>
VPC → Virtual Private Cloud → Internet Gateways → Create Internet Gateway\
Name tag: MyIGW 

Select MyIGW → Actions → Attach to VPC \
Available VPCs: select the VPC created in Step 1
\
\
**4) Create Route Tables and Associate the Subnets.** <br/>
VPC → Virtual Private Cloud → Route Tables \
Notice there is a Route Table here. This is the Main (default) Route Table! \
Feel free to rename (MainRouteTable)
\
\
VPC → Virtual Private Cloud → Route Tables → Create Route Table \
Name Tag: PublicRouteTable \
VPC: select the VPC created in Step 1

Select PublicRouteTable → Actions → Edit Subnet Associations \
Select the PublicSubnet (IPv4 CIDR 10.16.0.**1**/24)
\
\
\
Create another Route Table with the following settings \
Name Tag: PrivateRouteTable \
VPC: select the VPC created in Step 1

Select PrivateRouteTable → Actions → Edit Subnet Associations \
Select the PrivateSubnet (IPv4 CIDR 10.16.0.**2**/24)
\
\
**5) Add a Route to allow Internet traffic to the Public Subnets.** <br/>
With the PublicRouteTable selected, \
Routes (tab) → Edit Routes → Add Route\
Destination: 0.0.0.0/0 \
Target: select Internet Gateway, then MyIGW
\
\
**6) Clean up!!** <br/>
Virtual Private Cloud → Internet Gateways \
Select MyIGW → Actions → Detach from VPC \
Select MyIGW → Actions → Delete Internet Gateway

Virtual Private Cloud → Subnets \
Select the Public & Private Subnets → Actions → Delete Subnet

Virtual Private Cloud → Route Tables \
Select the Public & Private Route Tables → Actions → Delete Route Table \
(Don't worry about deleting the Main Route Table)

Virtual Private Cloud → Your VPCs \
Do not delete the Default VPC!! \
Select the VPC we created → Actions → Delete VPC \
(Deleting the VPC will also delete the Main Route Table)
