# Site2Site VPN Simple architecture with Virtual Private Gateway (slightly broken)

![Architecture](https://github.com/silverMatt92/aws-training/raw/master/Site2Site-VPN/VPN-architecture-fixed.png)

In this project you will implement a site to site VPN between AWS and a simulated on-premises business site.  
The Cloudformation yaml file provides the basic skeleton but the Site2Site VPN and the on-prem router configuration has to be done from scratch.

## Usage

- Create Site2Site VPN:
First of all, create the basic infrastructure using Cloudformation file
infra.yaml in this folder;
This will create the two VPCs: PICNIC-AWS and PICNIC-ONPREM, together with the
two private EC2 machines: AWS server and on-prem server.  
- You can either choose to deploy your on-prem router, selecting your favourite
  vendor from AWS Marketplace, or use the CloudFormation file
  Site2SiteVPN-strongswan-router.yaml, which deploys a simple Ubuntu EC2 with
  strongswan + FRRouting for BGP; If you choose the first option, please
  remember to launch the router into your public on-prem subnet, associate
  an Elastic IP to it, and configure a second ENI to speak with the private
  subnet;
- Create Customer Gateway Object;
- Create Virtual Private Gateway (VGW) and attach it to the AWS VPC;
- Create VPN connection;
- Download VPN connection configuration file and configure your router (either
  strongswan or your favourite vendor) accordingly; If you choose to use the
  yaml file with strongswan + FRRouting, please follow the section below;
- Test the private connectivity between the EC2 instances deployed.

## Strongswan IPsec + FRRouting BGP configuration

Connect to your Ubuntu EC2 using AWS Session Manager, change into root user and
cd into the /home/ubuntu/demo-assets/ directory.

`sudo bash`  
`cd /home/ubuntu/demo_assets/`

The IPsec configuation file to be edited is ipsec.conf, as well as the
ipsec.secrets for PSK configuration.
In order to utilize the route-based VPN solution, strongswan can use a script
that will bring UP the tunnel interfaces when needed.  
This is the file ipsec-vti.sh, that needs to be modified as per AWS VPN
configuration downloaded.  

Once the files have been modified correctly, copy them into the /etc folder and make the shell script executable with  

`chmod +x /etc/ipsec-vti.sh`

Now all the configuration for IPsec has been completed, lets restart the strongSwan service to bring them up.

`systemctl restart strongswan` to restart strongSwan ... this should bring up the tunnels

We can check these tunnels are up by running
`ifconfig`
You should see vti1 and vti2 interfaces
You can also check the connection in the AWS VPC Console ...the tunnels should be down, but IPsec should be shown as UP after a few minutes.

Now, in the same folder /home/ubuntu/demo_assets/, you have the shell script
that will install FRR, just make it executable and run it:  

`chmod +x ffrouting-install.sh`  
`./ffrouting-install.sh`  
** This will take some time - 10-15 minutes **

Once completed, you should be able to access the FRR terminal via  
`vtysh`

And from there, configure the BGP session and prefix advertisements:  
`conf t`  
`frr defaults traditional`  
`router bgp <AS-NUMBER>`  
`neighbor AWS_TUNNEL_IP_1 remote-as <AWS-AS-NUMBER>`  
`neighbor AWS_TUNNEL_IP_2 remote-as <AWS-AS-NUMBER>`  
`no bgp ebgp-requires-policy`  
`address-family ipv4 unicast`  
`redistribute connected`  
`exit-address-family`  
`exit`  
`exit`  
`wr`  
`exit`  
  
`sudo reboot`  

Once back, the linux router  will now be functioning as both an IPsec endpoint and BGP endpoint. 
It will be exchanging routes with the virtual private gateway in AWS.  

You can check the routes received with the commands:  
`vtysh`  
`show ip route`  

The final goal of the lab is to connect bidirectionally between the instances in the connected VPCs via their private IP addresses.  
Unfortunately, the lab is slighly broken, find the problem(s) and solve it!
