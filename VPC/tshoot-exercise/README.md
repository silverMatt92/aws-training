# Troubleshooting exercise
This is a troubleshooting exercise to test your basic VPC skills.  
Skills required:  
- VPC subnetting;  
- IGW and NAT-GW;  
- Route tables;  
- Security Groups;  
- NACLs;  
  
# Pre-requisites
An SSH key pair has to be available or created in us-east-1 AWS region.  
Download the private key .pem file and modify the permission as per documentation:  
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/create-key-pairs.html  

It's also easier to set up SSH forwarding to access the private EC2 instances via the Bastion.  
```bash
ssh-add -K name-of-your-key.pem
```
SSH into the Bastion host:
```bash
ssh ec2-user@public-ip-bastion
```
From there, you can simply SSH into the private EC2 instances:
```bash
ssh ec2-user@private-ip-ec2
```

# Usage
The yaml CloudFormation files deploy the architecture for you. A reference picture is provided.  
The files have to be used in sequence to create the stacks, because they depend on each other:  
1. Picnic-VPC-broken
2. Picnic-NAT-Gateways-broken.yaml  
3. Picnic-BastionHost.yaml  
4. Picnic-EC2-broken.yaml 
Please don't read the files otherwise you could understand what's broken in the architecture. 

# Objectives
The architecture is broken, solve the issues and make the followings working:  
1. Confirm the private EC2 instances are being natted with the EIP of the related NAT Gateways;  
2. Ping google.com from the private EC2;  
3. Ping between the two EC2 instances in each Availability Zone.
