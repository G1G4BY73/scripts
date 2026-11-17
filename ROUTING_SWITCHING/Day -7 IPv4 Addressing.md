#### Characteristics of network layer - Layer 3
- provides connectivity between end hosts on different networks. (i.e.. outside of the LAN)
- provides logical addressing (IP addresses)
- provides path selection between source and destination.
- Routers operate at Layer 3

# Routing 
- A router need an IP address, actually not just one IP address, it needs an IP address for each network it is connected to.
![[Pasted image 20250214203045.png]]

- This time, if PC1 sends a frame to the broadcast MAC address of all Fs. SW1 will receive the frame and it will forward it out of all of interfaces except the one frame was received on.
![[Pasted image 20250214203255.png]]

- So, it sends the frame out of G0/1 and G0/2, and PC1 and R1 receive the frame. however, that's where it ends.**The broadcast is limited to the local network**. it doesn't cross the router and go to SW2, PC3 and PC4. now will talk about router and Layer-3 forwarding later, ;here will learn first about IP address.

## IPv4 Header
![[Pasted image 20250214203953.png]]

We will  talk about all fields of IPv4 header but first let's talk about two important field. i.e. source IP address and destination IP address.

- These fields are both 32-bits in length, as you can see they stretch from 0 to 31 in this diagram.
![[Pasted image 20250214204041.png]]

#### CIDR
![[Pasted image 20250214204226.png]]

#### IPv4 Address Classes
![[Pasted image 20250214204627.png]]

>Addresses in Class D are reserved for 'multicast' addresses. Multicast is another type of address, separate from unicast and broadcast addresses.
 
> The end of the class A range is usually considered to be 126, Not 127.
>The 127 range is reserved for 'loopback addresses'. The range is anything with a first octet of 127,  so 127.0.0.0 to 127.255.255.255. These address are used to test the network stack.

- If a device sends any network traffic to an address in this range it is simply processed back up the TCP/IP stack as if it were traffic received from another device.
![[Pasted image 20250214205558.png]]

- Notice the round trip times, all 0 milliseconds, That's because the traffic isn't going anywhere, my PC is just sending and receiving these pings to and from itself.

![[Pasted image 20250214210437.png]]

![[Pasted image 20250214210755.png]]
![[Pasted image 20250214210844.png]]


