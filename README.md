**Title:** \
**Name:** Robert Mathisen\
**Date:** 3/11/2021 \
\
**Summary:** \
<br/>
**Reason & Reflection:** \
<br/>

**Process:** <br/>
\
**1) Create the VPC.** <br/>
VPC → Virtual Private Cloud → Your VPCs → Create VPC \
IPv4 CIDR block: 10.0.0.0/16 \
Tenancy: Default

**2) Create the Public & Private Subnets.** <br/>
VPC → Virtual Private Cloud → Subnets → Create Subnet\
VPC ID: select the VPC created in Step 1 \
Subnet Name: **Public**Subnet \
Availability Zone: us-east-1**a** \
IPv4 CIDR block: 10.0.**1**.0/24

VPC ID: select the VPC created in Step 1 \
Subnet Name: **Private**Subnet \
Availability Zone: us-east-1**b** \
IPv4 CIDR block: 10.0.**2**.0/24

**3) Create the Internet Gateway.** - allows you to connect the Public Subnet to the Internet! <br/>
VPC → Virtual Private Cloud → Internet Gateways → Create Internet Gateway\
Name tag: MyIGW 

Select MyIGW → Actions → Attach to VPC \
Available VPCs: select the VPC created in Step 1

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
Select the PublicSubnet (IPv4 CIDR 10.0.**1**.0/24)
\
\
\
Create another Route Table with the following settings \
Name Tag: PrivateRouteTable \
VPC: select the VPC created in Step 1

Select PrivateRouteTable → Actions → Edit Subnet Associations \
Select the PrivateSubnet (IPv4 CIDR 10.0.**2**.0/24)

**5) Add a Route to allow Internet traffic to the Public Subnets.** <br/>
With the PublicRouteTable selected, \
Routes (tab) → Edit Routes → Add Route\
Destination: 0.0.0.0/0 \
Target: select Internet Gateway, then MyIGW

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
