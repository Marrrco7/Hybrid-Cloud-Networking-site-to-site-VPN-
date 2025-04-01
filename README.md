
# Site-to-Site VPN connection between AWS and on-premises environment
-----

# Executive Summary

The present solution is meant for providing levarage between cloud infrastructure scalability and on-premises control of sensitive data. A site-to-site VPN was established in order to encrypt the communication
between the two segments of the built architecture, where both can communicate efficiently, reliably and in a secure manner, as if both environmens would be in the same local area network. The solution provides a nice degree of flexibility due to its hybrid architecture and on top of that, is meant to be improved for the future.
----
### For the full network configurations and implementation guide see the full guid pdf: 
----
# Architecture

![image](https://github.com/user-attachments/assets/45bb6ccc-9af7-4f27-b1ae-050b5f27b9a9)


As it is in the shown architecture diagram above, the architecture is divided into two main segments, the on-premises architecture and the cloud infrastructure handled by AWS.

## The architecture is composed of the following main components
-----

### On Premises Network (VirtualBox)
1. Ubuntu VM
   * The Ubuntu virtual machine serves as the main host behind pfSense, it will initiate communication with the cloud-based resources through the VPN tunnel.
2. PfSense
   * The pfSense virtual machine works as the router, DHCP server, NAT, firewall, and DNS provider for the Ubuntu VM. Apart from that, it provides the local area newtork for Ubuntu to connect to it, and most importantly, it establishes the ipSec VPN connection with AWS. It routes all the LAN traffic and enforces security rules that most be met for the inbound traffic.
3. LAN interface
   * The local area network interface acts as the local network provided by pfSense where the Ubuntu VM can connect. It provides traffic isolation for every device connected to it.
3. WAN interface
   * This network configured in pfSense via a bridge adapter, meaning it gets its own private IP address from the home router as if it was one more device in the newtork. It also acts as the public endpoint 
     (Customer Gateway) for the VPN tunnel.
     
### Cloud Environment (AWS)
1. VPC
   * The Virtual Private Cloud is an isolated network hosted by AWS that hosts all the cloud infrastructure where we can design and deploy our cloud network.
2. Private Subnets
   * The two private subnets are a logical subdivision of the cloud network, bounded by CIDR ranges, that are designed to host resources such as EC2 instances, and are only accessible troughout the VPN and not exposed to the internet. They provide security by isolating resoruces, and network better network management by improving network congestion and overall routing more efficient.
3. Customer Gateway (CGW)
   * The customer gateway is a representation of the on-premises pfSense firewall within AWS. It is defined by using the public IP of pfSense address of pfSense's WAN interface, which is the same as the public network IP.
4. Virtual Private Gateway (VPW)
   * AWS' managed concentrator (meaning it can connect to multiple VPN's) which is attached to the VPC and serves as the AWS endpoint for the VPN connection.
5. VPN connection
   * A site-to-site VPN tunnel that securely connects the the Virtual Private Gateway (AWS) with the on-premises Customer Gateway (pfSense). It uses static routing to allow the traffic between the VPC and the on-premises network.
6. Route table
   * The route table associated with the private subnets and is used to keep track of the paths and determines which way to forward the traffic (what gateways to use) is configured to send all traffic destined to the on-prem network IP through the Virtual Private Gateway, to provide proper routing across the VPN tunnel.

