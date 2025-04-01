
# Site-to-Site VPN connection between AWS and on-premises environment
-----

# Executive Summary

The present solution is meant for providing levarage between cloud inftrastructure scalability and on-premises control of sensitive data. A site-to-site VPN was established in order to encrypt the communication
between the two segments of the built architecture, where both can communicate efficiently, relicably and in a secure manner, as if both environmens would be in the same local area network. The solution provides a nice degree of flexibility due to its hybrid architecture and on top of that, is meant to be improved for the future.



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

