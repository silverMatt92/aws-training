# Troubleshooting exercise

![Architecture](https://github.com/silverMatt92/aws-training/raw/master/VPC/tshoot-exercise/tshoot-exercise.png)

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
Connect to the Bastion host using System Manager via AWS EC2 Console and from there, first switch to the ec2-user:
```bash
sudo su - ec2-user
```
From there, you can simply SSH into the private EC2 instances:
```bash
ssh -i aws-training-picnic.pem <private-ip-ec2>
```

# Usage
The yaml CloudFormation files deploy the architecture for you. A reference picture is provided. Use it in us-east-1 region.  
Open the CloudFormation console in AWS, in us-east-1 region, then:
- Create stack  
- Under "Specify template", select "Upload a template file"  
- Choose the yaml file and wait until it says CREATE COMPLETE    
This same process has to be repeated for each yaml file in the following sequence, because each one depends on the succseful deployment of the previous:  
1. Picnic-VPC-broken.yaml
2. Picnic-NAT-Gateways-broken.yaml  
3. Picnic-NACLS-broken.yaml
4. Picnic-BastionHost.yaml  
5. Picnic-EC2-broken.yaml  

Currently, the lab can be deployed in the following regions:  
- us-east-1 N.Virginia
- eu-central-1 Frankfurt
- eu-west-3 Paris
- eu-west-2 London
- sa-east-1 Sao Paolo

The files have to be used in sequence to create the stacks, because they depend on each other.  
Whenever it asks to provide the key-pair to use, please select the "aws-training-picnic" one.    

Please don't read the files otherwise you could understand what's broken in the architecture.  
Once done, remember to delete the stacks in the opposite order w/r of their creation, to avoid paying for something you don't use.

# Objectives
The architecture is broken, solve the issues and make the followings working:  
1. Confirm the private EC2 instances are being natted with the EIP of the related NAT Gateways;  
2. Ping google.com from both private EC2 in each Availability Zone;  
3. Ping between every EC2 instances.
