# NAT Gateway Deep Dive
This short article will show and explain the behaviour of NAT Gateway because
many people have not completely clear how it works and where the NAT is
actually happening whenever an instance is trying to reach the internet via the
NAT-GW or directly via the Internet Gateway.

# Packet walk

![Architecture](https://github.com/silverMatt92/aws-training/raw/master/VPC/NAT-Gateway-Deep-dive/NAT-GW-drawing.png)

Imagining a TCP communication between the private clients (using the same TCP source port) depicted and a public
server, listening on port 8080, here's the detailed packet walk:

1. EC2 clients craft the TCP segment with source port 32222 and destination 8080,
   directed to the public server;
   EC2 instance route table will be looked up, and the default route is
   matching the default gateway for the private subnet 10.0.1.1;
   Subnet route table is looked up, showing that default route must be sent to
   the NAT-GW nat-xxx;
2. NAT-GW ENI receives the packets, and performs two simple steps: source
   IP/source port translation and routing;
3. Translation table gets populated and the source IP gets natted to the
   private IP associated to the NAT-GW ENI, in this case 10.0.0.226;
   Route table where NAT-GW is deployed is looked up, and default route is
   pointing towards the Internet Gateway igw-yyy;
4. Traffic reaches the IGW and here the real 1:1 static NAT happens, which
   translated the private IP of NAT-GW ENI, with its associated Elastic IP, in
   this case 3.224.9.52; Please note that IGW does not perform any actions on
   the ports at Layer 4, but only performs the static 1:1 NAT between private
   and public;
5. Traffic gets routed through the internet;
6. Traffic reaches the server and it replies back;
7. Traffic back reaches the IGW, which applies the 1:1 mapping backwards so
   that the destination is the NAT-GW ENI private IP and
   based on the main VPC route table, routes the packets locally;
8. Packets gets translated back by the NAT-GW, based on its translation table
   and get routed locally to reach back the clients.


![Architecture](https://github.com/silverMatt92/aws-training/raw/master/VPC/NAT-Gateway-Deep-dive/clients-server.png)
