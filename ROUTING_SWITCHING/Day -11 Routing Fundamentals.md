#### What is routing ?
- Routing is the process that routers use to determine the path that IP packet should take over a network to reach their destination.
- Router store routes to all of their known destination in a routing table.
- When router receive packets , they look in the routing table to find the best router to forward that packet.

### There are two main Routing Methods (Methods that routers use to learn routes)

#### Dynamic Routing
- Routers use dynamic routing protocols (i.e. OSPF) to share routing information with each other automatically and build their routing tables.

#### Static Routing 
- A network engineer/admin manually configures routes on the router.

*A **Route** tells the router  to send packet to the destination X, you should send the packet to next-hop Y, or, if the destination is directly connected to the router, send the packet directly to the destination, or, if the destination is the router's own IP address, receive the packet for yourself and not forward it*

Lets understand with the help a  small example.
![[Pasted image 20250221164718.png]]

>Here first we will learn about the two types of routes automatically added to a router's routing table, but these are not dynamic or static routes these are default you can say.

- Before looking at routes Lets configure the IP addresses of the routers.
**R1** 
![[Pasted image 20250221165055.png]]

**Lets take a look at R1 routing table**
```
Show ip route 
```

![[Pasted image 20250221170750.png]]

Earlier we motioned there are two main type of routing dynamic routing and static routing, these connected and local routes are neither they are simply added to router when a interface is configured.

These are the two routes we will focus in this session.

![[Pasted image 20250221171027.png]]
## Connected Route 
![[Pasted image 20250221171249.png]]
![[Pasted image 20250221172954.png]]

## Local Route
![[Pasted image 20250221171323.png]]
![[Pasted image 20250221173038.png]]


# Route Selection
![[Pasted image 20250221173603.png]]

![[Pasted image 20250221190403.png]]

**Router never floods packet** if the router doesn't have a route to the packet's destination, it will drop the packet.

### summary 
- Routers store information about destination they know in their routing table.
- Each route in the routing table is an instruction.
	- To reach destination in network X, send to packet to next hop Y (the next router in the path to the destination)
	- If the destination is directly connected (connected route) send the packet directly to the destination.
	- If the destination is your IP address (local route), receive the packet for yourself.
- When you configure an IP address on an interface and enable the interface, two routes are automatically added to the routing table:
	- Connected route (code C in the routing table): A route to the network connected to the interface.
	- Local route (code L in routing table) : A route to the exact IP address configured on the interface.
- A route matches a destination if the packet destination IP address is the part of network specified in the route.
- If a route receives a packet and it doesn't have a route that matches the packets destination, it will drop the packet.
- If a router receives a packet and it has multiple routes that match the packet's destination, it will use the most specified matching route to forward the packet.
