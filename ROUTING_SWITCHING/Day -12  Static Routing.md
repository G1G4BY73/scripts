> In this last lesson we looked two types of routes: local routes and connected routes.
>  Those routes are automatically added to a router's routing table when you configure an IP address on a interface.
>  Local routes provide a route to the route's own IP address, and connected router provide a route to the network the interface is connected to.

However, if you want the router to be able to send traffic to destination that aren't directly connected to itself, Local and connected routes aren't enough. 

Here we will learn how static route can enable sending a traffic to remote destination that are not directly connected to router itself.

## Static Routing 

##### Let's Take the last Example
![[Pasted image 20250223190135.png]]

- Configuring the IP address of Routers
R2 IP configuration
![[Pasted image 20250223190554.png]]
> Likewise for all routers need to configure IP address.

- Lets check the routing table
![[Pasted image 20250223193922.png]]

here, R2 know how to reach its own IP addresses and destination in its connected networks, but it doesn't know how to reach destination in remote network. remote network means network not directly connected to R2.

- So, R2 knows how to reach destination in the 192.168.12.0/24 and 24.0/24 networks, including its own IP addresses in those networks. but it doesn't know how to reach 1.0/24, 4.0/24, 13.0/24 and 34.0/24
![[Pasted image 20250223200113.png]]

R3 Configuration
![[Pasted image 20250223200202.png]]

![[Pasted image 20250223200226.png]]

R4 Configuration
![[Pasted image 20250223200300.png]]

![[Pasted image 20250223200317.png]]


If R4 wants to send a packet destined for PC4 then it can, but if it wants to send to PC1  192.169.1.10 

### Default Gateway
 - End host like PC1 and PC4 can send packets directly to destination in their connected network.
	 - PC1 is connected to 192.168.1.0/24, PC4 is connected to 192.168.4.0/24.
- To send packet to destination outside of their local network, they must send the packet to their default gateway.
*Default gateway is default router*

Configuration on PC1 and PC4
![[Pasted image 20250228231807.png]]
![[Pasted image 20250228231818.png]]

![[Pasted image 20250228231907.png]]

- The default gateway configuration is also called a default route.
	-  It is route to 0.0.0.0/0 = all netmask bit set to 0. Include all address from 0.0.0.0 to 255.255.255.255.
- ![[Pasted image 20250228232151.png]]
- End hosts usually have no need for any specific routes.
	- They just need to know : to send the packet outside of my local network, I should send them to my default gateway.
-  Now PC1 want to send a packet to PC4. so the IP header is having.
![[Pasted image 20250228232521.png]]

- Now R1 received the Frame it will de-encapsulate it. meaning it will remove the Layer2 header and trailer and look at the inside packet. It will then check the routing table for the most-specific  matching route. Here is R1's routing table at the moment, just connected and local routes.
![[Pasted image 20250228232845.png]]

- But R1 has no matching routes in its routing table. hence it will drop the packet. In its current state, R1 doesn't know how to properly forward the packet. To forward it R1 needs a route to the destination network, 192.168.4.0/24.
- Routes are instructions, R1 needs a route to say: to send a packet to destination in network 192.168.4.0/24, forward the packet to next hop Y.
- Now we know the destination, the 4.0/24 network. but what should be the next hop ?
- What is the next router in the path to the destination?
- Actually there are two possible paths that packet from PC1 to PC4 can take.
	- PC1-> R1-> R3 -> R4 -> PC4
	- PC1 -> R1 -> R2 -> R4 -> PC4
- Here we will use path Via R3, not the path via R2.
![[Pasted image 20250228233818.png]]

![[Pasted image 20250228233854.png]]

- Each router in the path needs two routes : a route to 192.169.1.0/24 and a route 192.168.4.0/24.
	- The ensure two-way reachability(PC1 can send packet to PC4, PC4 can send packet to PC1)
![[Pasted image 20250228234357.png]]
- R1 already has a connected route to 192.168.1.0/24. R4 already has a connected route to 192.168.4.0/24.
	- The routes must be manually configured(using static routes)'
![[Pasted image 20250228234751.png]]

We've planned the static routes we need to configure.

- To allow PC1 and PC4 to communicate over the network, let's configure these static routes on R1, R3 and R4.
- Lets first configure R1's route 
![[Pasted image 20250228235156.png]]
here is the format for the command.
```
R1(config)# ip route ip-address netmask next-hop
```

![[Pasted image 20250228235332.png]]
> check wheatear route is added or not.

```
do show ip route
```

![[Pasted image 20250228235515.png]]

![[Pasted image 20250228235526.png]]

 ![[Pasted image 20250228235615.png]]
 ![[Pasted image 20250228235627.png]]
- So, we have configured all of the static routes necessary for PC1 and PC4 to communicate. lets test it out and see if they can communicate with each other.
- To check lets ping PC4 from PC1.
![[Pasted image 20250228235848.png]]

![[Pasted image 20250228235901.png]]

![[Pasted image 20250301000045.png]]

## Another Ways of configuring static route

- when configuring static route instead of configuring a next-hop you can configure an exit-interface instead.
- That means you specify which interface this router should send the packet out of, rather then telling it the actual IP address of the next-hop

**For Ex:**
 If R2 wants to send packets to the 192.168.1.0/24 network, it should send the packet to R1. what interface should should R2 send packets out of to reach R1?
 - G0/0 because that interface is connected to R1.
```
R2(config)# ip route 192.168.1.0 255.255.255.0 g0/0
```
- Also you can specify both the exit interface and the next hop. So if R2 wants to send packet to the 192.168.4.0/24 network, it should send packets to R4.
- And which interface should it send the packets out of to reach R4? G0/1, because its connected to R4. And what is the next hop IP address? it's 192.168.24.4, the IP of R4's G0/0 interface.


