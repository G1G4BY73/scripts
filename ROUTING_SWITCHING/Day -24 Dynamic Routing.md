![[Pasted image 20250330235912.png]]

=> Topics to cover
+ Dynamic routing
+ Types of Dynamic routing
+ Routing protocol metrics
+ Administrative distance

![[Pasted image 20250331000143.png]]

=> Without configuring static routes or dynamic routing protocol, R1's routing table looks like this, just connected and local routes.
![[Pasted image 20250331000257.png]]

Which were automatically is added when IP addresses were configured on its interfaces.

- Here 10.0.12.0/30 and 10.0.13.0/30 are examples or network routes. A network route is simply a route to a network or subnet, In other words, a route with mask length less then /32.
![[Pasted image 20250331000522.png]]

![[Pasted image 20250331000533.png]]

- 10.0.12.1/32 and 10.0.13.1/32, are example of host routes. A host route is a route to a specific host, a single address, specified with a /32 mask. These two routes here  automatically added, and are host routes to the specific addresses configured on R1's G0/0 and G0/1 interfaces. To configured a static route to a host, use `ip route`, followed by the host address, then 255.255.255.255, which is /32 mask.
![[Pasted image 20250331000957.png]]

---
### Dynamic Routing.
=> instead of configuring static routes on each of these routes, we can enable a dynamic routing routing protocol on them, Then R4 will advertise this 192.168.4.0/24 network to its neighbour, R2 saying you can reach this network via me.
![[Pasted image 20250331001411.png]]
=> R2 will add that route to its routing table. And It will advertise the same thing to R1 telling R1 that it can reach 192.168.4.0/24 via R2. R1 will add this route to its route table. 
![[Pasted image 20250331001642.png]]
=>actually, if you look above the route, you can see that R2 also advertise the 10.0.24.0/30 network, between R2 and R4, to R1, and R1 added it to its route table. R1 will then in turn advertise to R3, telling R3 that it can reach 192.168.4.0/24 via R1. it will also advertise the 10.0.24.0/30 network that it learned from R2, as well as the 10.0.12.0/30 network between R1 and R2, but we're just focusing on the one network for now.

- How about if there is an error and R4's G0/0 interface goes down? The other routes will automatically adapt and remove the route from their route table. 
![[Pasted image 20250331002058.png]]

- As you can see R1 has remove the route. This will prevent R1 from continuously sending traffic to a dead-end.
![[Pasted image 20250331002201.png]]

*What if the same situation happened when using static routing ?*

> Because there is no routing protocol in use, R1 is unaware that is can no longer reach the 192.168.4.0 network. If it will received a packet destined for that network, it will continue forwarding them to R2, unaware that R2 can no longer reach the network.

**That's a benefit of dynamic routing, The router will remove the invalid routes**

=> however we really should make sure that, there is a backup route. so instead of totally remove the destination network from the route table. it is replaced with next best route.

- SO, now I have added another connection between R3 and R4. Now R1 has two valid paths to R4's internal network
![[Pasted image 20250331134536.png]]


=> via R2, and Via R3. Lets check R1's route table. as you can see, the route via R2 in its route table, as it says via  10.0.12.2  in this case, what will happen if I disable R4's G0/0 interface to simulate  a failure ? 
![[Pasted image 20250331143157.png]]

=> So, I did that, and now let's check R1's route table as you can see, the route via R2 was now automatically replaced with route via R3. it says via 10.0.13.2

![[Pasted image 20250331143652.png]]

=> So, we lost the preferred route to 192.160.4.0/24, but traffic can still follow this path. Now you may be wondering why the route R2 was preferred over the route via R3? that's because this connection here is a fast ethernet, not gigabit ethernet.

![[Pasted image 20250331144040.png]]

*Things to Remember*

![[Pasted image 20250331144442.png]]

#### Lets Talk about different routing protocol

=> Dynamic routing protocol  can be divided  into two main categories.
 1. IGP (Interior Gateway Protocol)
 2. EGP (Exterior Gateway Protocol)
![[Pasted image 20250331145451.png]]

----
![[Pasted image 20250331150603.png]]

# Distance Vector Routing Protocol

- Distance vector protocols were invented before link state protocol.
- Early examples  are RIPv1 and Cisco's  propriety protocol IGRP (which was updated to EIGRP)
- Distance vector protocols operated by sending the following to their  directly connected neighbors:
 => Their knows destination networks.
 => Their matric  to reach their known destination networks

- This method  of sharing route information is often  called "routing by rumor"
- This because the router doesn't know  about the network beyond its neighbors it only knows the information that its neighbors tell it.
- Called 'distance vector' because the router only learns the 'distance' (matric)  and vector(direction, to next hop router) of each route.


---
- The example we have see before of R4 advertising its 192.168.4.0 network is an example of distance vector logic, R4 tell R2 its directly connected neighbor, you can reach 192.168.4.0/24 via me. My metric to reach it is 1.
- R2 doesn't know anything expect that it can reach 192.168.4.0/24 via R4. and that R4 matric is 1. similarly, R2 tells R1 the Same, except it advertises the matric as 2. once again R1 doesn't have a dedicated picture of the network, all it know is that it can reach 192.168.4.0/24 via R2 and that R2's matric is 2. 
- And of course, R1 advertises the network to R3, with its own matric to reach the destination network. RIP and EIGRP  are the two distance vector routing protocols that are used.
![[Pasted image 20250331153354.png]]

# Link State Routing Protocol
![[Pasted image 20250331203450.png]]

### Dynamic routing protocol Metrics
![[Pasted image 20250331203620.png]]


=> Here in the below slide R1 learns two paths to 192.168.4.0/24, one via R2 an done via R3, only the route via R2 is added to the routing table. The fastethernet connection here has a higher metric cost than the other gigabit ethernet connections, so this route is less favourable.
![[Pasted image 20250331204315.png]]

=>  Now, you might be wondering: what if this was also a gigabit ethernet connection? Both routes would have same cost, so what route would be added to the route table?
Lets change the connection b/w R3 and R4 to be a gigabit ethernet connection like the others. lets check R1's routing table.
![[Pasted image 20250331204526.png]]

=> So, both routes have been added to routing table. via 10.0.13.2 which is R3 and 10.0.12.2, which is R2. So.
![[Pasted image 20250331204712.png]]

![[Pasted image 20250331204829.png]]

*Highlighted red is the metric value of 3, both are having the metric value of 3 so both are added and traffic will be load-balanced over both the routes.*

*Another value in square brackets of 110 is also an important value called administrative distance*
**This is called ECMP Load-balancing**
![[Pasted image 20250331205019.png]]

=> The OSPF protocol has  an AD of 110.
***You can also do the ECMP with static routing***

![[Pasted image 20250331205318.png]]

*here both routes have a metric of 0. static routes don't really use the concept of 'metric' so you'll always see 0 here. Notice also that the administrative distance AD is 1*

## Metric 
=> Each routing protocol uses a different Metric, which is a value used to determine the best route to a destination.
=> Each Routing protocol use a different metric. 
![[Pasted image 20250331205653.png]]

EX. 
![[Pasted image 20250331205924.png]]

=> However, how would you compare which the same protocol.
### Administrative Distance
![[Pasted image 20250331205947.png]]

![[Pasted image 20250331210141.png]]


##### Administrative Distance for Most common type of routes
![[Pasted image 20250331210244.png]]
*These are the Value used on Cisco Devices. other vendors might rack these differently*

  ![[Pasted image 20250331210618.png]]
##### Example : 
![[Pasted image 20250331210801.png]]

![[Pasted image 20250331210952.png]]

![[Pasted image 20250331211021.png]]

![[Pasted image 20250331211116.png]]

![[Pasted image 20250331211125.png]]

