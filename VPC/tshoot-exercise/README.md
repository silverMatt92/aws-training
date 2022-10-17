# Troubleshooting exercise
This is a troubleshooting exercise to test your basic VPC skills.  
Skills required:  
- VPC subnetting;  
- IGW and NAT-GW;  
- Route tables;  
- Security Groups;  
- NACLs;  
  
# Pre-requisites
There's already an SSH key pair created, called aws-training-picnic and this key is automatically 
added to the bastion host under /home/ec2-user directory.
From the bastion, first switch to the ec2-user:
```bash
sudo su - ec2-user
```
From there, you can simply SSH into the private EC2 instances:
```bash
ssh -i aws-training-picnic.pem <private-ip-ec2>
```

# Usage
The yaml CloudFormation files deploy the architecture for you. A reference picture is provided. Use it in us-east-1 region.  
Consder that, due to limitation in AWS Elastic IP number (5 per account per region) it won't be possible to launch the templates 
if someone else already deployed it.
Please check if any stack is already deployed before attampting to create them. 
The files have to be used in sequence to create the stacks, because they depend on each other:  
1. Picnic-VPC-broken.yaml
2. Picnic-NAT-Gateways-broken.yaml  
3. Picnic-NACLS-broken.yaml
4. Picnic-BastionHost.yaml  
5. Picnic-EC2-broken.yaml  

Please don't read the files otherwise you could understand what's broken in the architecture.  
Once done, remember to delete the stacks in the opposite order w/r of their creation, to avoid paying for something you don't use.

# Objectives
The architecture is broken, solve the issues and make the followings working:  
1. Confirm the private EC2 instances are being natted with the EIP of the related NAT Gateways;  
2. Ping google.com from both private EC2 in each Availability Zone;  
3. Ping between every EC2 instances.
