# Site2Site VPN Simple architecture with Virtual Private Gateway

![Architecture](https://github.com/silverMatt92/aws-training/raw/master/Site2Site-VPN/VPN-architecture-fix.png)

In this project you will implement a site to site VPN between AWS and a simulated on-premises business site.  
The Cloudformation yaml file provides the basic skeleton but the Site2Site VPN and the on-prem router configuration has to be done from scratch.

## Usage

- Create Site2Site VPN:
First of all, create the basic infrastructure using Cloudformation file
infra.yaml in this folder;
This will create the two VPCs: PICNIC-AWS and PICNIC-ONPREM, together with the
two private EC2 machines: AWS server and on-prem server.  
- Reserve an Elastic IP to be used by your simulated on-prem router;
- Deploy your favourite on-prem router vendor into the PICNIC-ONPREM public
  subnet 192.168.12.0/24, remember that this will need a second interface into
  the private subnet as well;
- Create Customer Gateway Object;
- Create Virtual Private Gateway (VGW) and attach it to the AWS VPC;
- Create VPN connection;
- Download VPN connection configuration file;
- Test the private connectivity between the EC2 instances deployed.
