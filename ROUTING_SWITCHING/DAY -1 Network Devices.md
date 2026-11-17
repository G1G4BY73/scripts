
> Jeremy's IT Lab CCNA 200-301


###  What is Computer Network ?  
A Computer Network is a digital telecommunication network which allows nodes to share resources.

## Network Devices.
![[{BA60B4C8-AD8E-4985-9F8F-3918427855D6}.png]]

> A Client is a device that  accesses a service made available by a server.
> A server is a device that provides functions or services for clients.

The same device can be a client in some situations, and a server in other situations.

### Switches 
> Catalyst 9200(48ports), Catalyst 3650(24ports)

- Switches have many network interfaces/ports for end hosts to connect to. 
- Provides connectivity to hosts  within the same LAN (Local Area Network)
- Do not provides connectivity between LANs/over the internet.

![[{C6237D5D-7E04-4CEA-871D-07EFA63E6057}.png]]


### Routers 
> ISR 1000, ISR 900, ISR 4000

A router is a device that is designed to connect and forward network traffic between multiple networks.

- Have Fewer network interfaces than switches.
- Are used to provide connectivity between LANs.
- Are therefore used to send data over the Internet.

### Firewalls
> ASA5500-X, Firepower 2100

Although Router can also provide some basic security features. but we what should really be using to protect our network is A Firewall. Firewalls are specialty network security devices that control  traffic entering and exiting your network. Firewall can be placed outside of your router like FW1, or inside of your network like FW2. What's important is that they protect the hosts inside like PCs and Servers in the network.

Firewall must be configured with security rules to determine which network traffic should be allowed and what  should be denied. If you configure the rules properly, If PC1 in the New York branch   tries to access SRV1 in Tokyo, the firewall should permit the traffic through. the return traffic from the SRV1 to PC1 should be allowed as well. However, if the attacker tried to access anything inside your network, the firewall should block it.  

![[{9C6CEC37-4E3F-40E0-89F2-A9E45A838E6E}.png]]

- monitor and control network traffic based on configured rules.
- can be placed inside the network, or outside the network.
- are known as 'Next-Generation Firewalls'  when they include more modern and advanced filtering capabilities.

#### What about the firewall configured on your computer ?
###### Network Firewalls
are the hardware devices that filters traffic between networks. 
###### Host-based Firewalls

> Your PC almost certainly has a software firewall installed, as it should. Even in a network with a hardware firewall, each PC should include a software firewall as an extra line of defence.

