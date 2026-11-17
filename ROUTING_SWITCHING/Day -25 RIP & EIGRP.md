
### Routing Information Protocol (industry standard)
- RIP stands for Routing Information Protocol, and it is a industry standard protocol, not cisco proprietary.
- It is a Distance Vector IGP so, it Uses routing-by-rumour logic to learn/share routes.
- RIP uses hop count as its metric, each router in the path to the destination count as one hop. and the bandwidth is irrelevant.
- The maximum hop count is 15 and Anything more than that is considered unreachable, and RIP will not insert the route into the  routing table.
- So, clearly RIP cannot be used for a very large networks.
- Actually, RIP is almost never used in real networks, bur it can possibly be used in networks, and also in lab environments, as a simple IGP that is quick and easy to set up.
- RIP has Three version :  RIPv1 and RIPv2 used for IPv4, RIPng(RIP next generation), used for IPv6.
- RIP uses two message type to learn and share routing information.
=> **Request** : To ask RIP-enabled neighbour routers to send their routing table.
=> **Response** : To send the local router's routing table to neighbouring routers.
- By default, RIP-enabled routers will share their routing table every 30 seconds.

![[Pasted image 20250331214017.png]]

*This can cause problems in network with lots of routers, as these regular update can clog up the network*

#### Lets Compare RIPv1 and RIPv2
![[Pasted image 20250402001238.png]]

### Basic Configuration of RIP 
- RIP configuration is very simple, so it acts as a good introduction to dynamic routing configuration.
- Some of the mechanics are similar to how OSPF configuration works, so it will make it easier when we go in depth on OSPF later.
- So, Assuming all of the other router have already set up with RIP, let's just  configure R1.
![[Pasted image 20250402001624.png]]

- First Enter RIP configuration mode. with command.
```
router rip
```
- Next, configure the router with RIP version 2. with the command.
```
version 2
```
- This isn't necessary, but you should always use version 2 if you're going to use RIP. cause Classful IPv4 addressing is a thing of past, in modern network we need to be able to things like VLSM and CIDR.
- Then, use command no auto summary. Auto-summary is on by default and it automatically convers the networks the router advertises to classful networks.
- For example, using classful logic, the 172.16.1.0/28 network attached to R1 is a class B network, so it would be advertised as 172.16.0.0/16. 
- Always use these two command when you configure RIP, version 2 followed by no auto-summary
```
no auto summary
```
- Next up, we have to use the network command. First I use network 10.0.0.0- This command itself is classful, it will automatically convert to classful networks. for ex example, even if you enter the command. network 10.0.12.0, it will be converted to network 10.0.0.0(a class A network). so /8 prefix length is assumed. so after the first 8 bits all the other bits will be converted to 0.
- Because of this behaviour, there is no need to enter a network mask.
- Okay, so what effect does this command actually; have? 
- R1's G0/0 interface is 10.0.12.0/30, and its G1/0 interface is 10.0.13.0/30, but I just entered the command Network 10.0.0.0

![[Pasted image 20250402004752.png]]

=> Let's look at how the `network` command functions. 
- The network command tells the router to:
	- look for interfaces with an IP address that is in the specified range
	- active RIP on the interfaces that fall in the range.
	- it forms adjacencies with other connected RIP-enabled neighbours, and advertise the network prefix of the interface.(Not the prefix in the network command)

### Let's See this in action.

![[Pasted image 20250402001624.png]]

So, we just entered `network 10.0.0.0` command on R1.
![[Pasted image 20250402005216.png]]
- Because the network command is classful, 10.0.0.0 is assumed 10.0.0.0/8
- R1 will look for any interfaces with an IP address that matches 10.0.0.0/8 (because it is /8 it only need to match the first 8 bits.)
- So the first octet of the IP address needs to be the same.
- 10.0.12.1 and 10.0.13.1 both match, so RIP is activated on G0/0 and G1/0 
- R1 then forms adjacencies with its neighbours R2 and R3.
- R1 will send and receive route information to and from R2 and R3. 
- Here's the important part, R1 advertises 10.0.12.0/30 and 10.0.13.0/30, the network prefixes of its G0/0 and G1/0 interfaces, to its RIP neighbours, R1 and R3
- Even Though we used the network 10.0.0.0 command, R1 doesn't advertise the 10.0.0.0/8 network.

![[Pasted image 20250402010036.png]]

Okay, We also configured the `network 172.16.0.0` command. Let's run through this as well.
- Because the network command is classful, 172.16.0.0 is assumed to be 172.16.0.0/16.
- R1 will look for any interfaces with an IP address that matches 172.16.0.0/16. 
- 172.16.1.14 matches, so R1 will activate RIP on G2/0.
- This time no RIP neighbours connected to G2/0, so no new adjacencies are formed.
- However R1 advertises 172.16.1.0/28 (Not 172.16.0.0/16) to its RIP neighbours.

*One important point.*
![[Pasted image 20250402010653.png]]
> Lets see how to do that.
```
passive-interface G2/0
```
![[Pasted image 20250402010753.png]]

> Note that command is done from RIP configuration mode, not directly on the interface itself.

- The `passive-interface` commands tells the router to stop sending RIP advertisements out of the specified interface (G2/0)
- However, the router will continue to advertise the network prefix of the interface (172.16.1.0/28) to its RIP neighbours (R2, R3).
- you should always use the `passive-interface` command on interfaces which doesn't have any RIP neighbours.
- EIGRP and OSPF both have the same functionality, using the same command.

**To demonstrate one more function of RIP I added an Internet connection to R1, via it's   G3/0 interface**

- Then I configured  a default route pointing to the internet. So any packet that doesn't match any of the other entries in R1's routing table will be sent to the internet.
![[Pasted image 20250402012017.png]]
![[Pasted image 20250402012158.png]]
![[Pasted image 20250402012053.png]]

> Now, I want to use RIP to tell R2, R3 and R4 about this default route so that they can reach the internet as well.

- The command to share this default route into RIP is `default-information originate`, again the command is done from RIP configuration mode.
![[Pasted image 20250402012545.png]]

- Now that I have entered this command, R1 will advertise the route to R2 and R3, and they will then advertise it to R4.
- Let's just check the routing table to R4.

![[Pasted image 20250402012716.png]]

- Notice, it says gateway of last resort is 10.0.34.1 to network 0.0.0.0, however below that you can see two routes, one via F2/0 and R3 and one via G0/0 to R2.
- Only one is actually stated up to as being the gateway of last resort, but because both of these routes have the same hop count, R4 will load balance traffic over the two routes.
- RIP treats all connections equally, as one hop, so even though the connection via R3 is a slower fast Ethernet connections, RIP considers it equal to the  faster gigabit connection via R2. 
- OSPF also has the same `default-information originate`, command by the way, to share default route with neighbours.

-----
*Now let's take a look at very useful show command, show IP protocol*

- This command can be used for RIP, EIGRP, and OSPF to check various stats.
- First up, this part here identifies the protocol that is being used, RIP in that case.
![[Pasted image 20250402120432.png]]

- These blow that are the timers that RIP uses to operate, we won't take much about here for RIP and EIGRP but will go in depth when we study  OSPF.
- Here is some information about the version being used, notice that's it's version 2 as we configured.
![[Pasted image 20250402120845.png]]
-  Automatic network summarization is not in effect, this is because we used the No auto-summary command earlier.
- Below that Maximum path is 4 which is refers to ECMP load balancing, By default RIP will insert up to 4 paths to same destination into the routing table. if they have the same metric.
- However, this can be changed. here is the command `Maximum-Path` followed by a number, 1 to 32.
![[Pasted image 20250402122245.png]]
> this should be done from RIP config mode.
- This command is the same for EIGRP and OSPF  also by the way.
- Next up, this section here shows the network we entered  with the `network` command.
![[Pasted image 20250402122642.png]]
- Once again, these aren't the actual networks RIP is adverting, the network command just identifies  which interfaces to active RIP on.
- Below that you can see any passive interfaces listed, just G2/0 in this case.
- Under `routing information source` you can see R1's RIP neighbors, 10.0.12.2 which is R2 and 10.0.13.2 which is R3.
- Finally, Distance state the administrative distance of RIP, which is currently the default of 120. This can be changed with the command from RIP configuration mode, Distance, followed by a number, 1 to 255.
![[Pasted image 20250402123743.png]]

- For example, if you want to make RIP routes preferred over EIGRP routes for some reason, you could set it to 85 like i just did, to make RIP's AD lower than EIGRP's AD of 90.
- The distance command is the same for EIGRP and OSPF also, by the way.

________

# EIGRP 
=> stands for Enhanced Interior Gateway Routing protocol.

- It is an improved version of the older IGRP. Interior Gateway Routing Protocol.
- EIGRP was Cisco proprietary, but Cisco has now published it openly so other vendors can implement in on their equipment.
- However, my understanding is that Cisco didn't open up the entire protocol. parts of it remain Cisco-proprietary, and I don't think many vendor bothered implementing EIGRP. So, practically speaking, it's still considered a Cisco-only protocol.
- It is considered an 'advanced/ hybrid' distance vector routing protocol.
- It improves on the very basic operations of RIP, its fellow distance vector routing protocol.
- It is Much faster then RIP in reacting to change in the network.
- It doesn't have the 15 hop-count limit of RIP so it supports very large network.
- It sends messages using multicast network address 224.0.0.10.
> Remember, RIPv1 broadcast messages, and RIPv2 multicast them to 224.0.0.9 
- EIGRP multicast them to 224.0.0.10 
- Finally one unique feature of EIGRP, EIGRP is the only IGP that can perform unequal-cost load-balancing. (by default it performs ECMP load balancing over 4 path like RIP), but you can configure it to load balance over multiple path that don't have equal cost.
- EIGRP will even load-balance them in proportion to their bandwidth. So more traffic will be sent over paths with a lower-metric, since they are faster, and less traffic will be sent over path with a higher metric, since they are the slower paths.
- EIGRP is a great protocol, but because its use in most limited only to Cisco devices, it's not used nearly as much as OSPF is.
- That's why cisco made OSFP the main focus on the new CCNA when it comes to dynamic routing protocols.

## EIGRP Configuration...
=> here is the same network topology used for RIP configuration, here RIP configuration is removed, although you can have both the configuration running at same time. But that would just be a waste of resources on the routers, so usually there will just on IGP running on a router.

![[Pasted image 20250403205654.png]]

- Enter EIGRP configuration mode with command `router eigrp` followed by an AS, autonomous system number, I used 1.
- The AS number must be match between router or they will not form an adjacency and share route information. I already configured the same AS number of 1 on R1,2,3 and 4, so I had to configure the same here on R1.
![[Pasted image 20250403210047.png]]

- Next, disable the auto-summary. It functions same as RIP, it will advertise classful network instead of the actual network prefix configured on its interfaces.
![[Pasted image 20250403210441.png]]

- Then used the same `passive-interface` command as we did for RIP.
- Next up we used the `network 10.0.0.0` command to activate EIGRP on the G0/0 and G1/0 interfaces.
![[Pasted image 20250403210756.png]]

- You can use a mask with EIGRP's Network command, however it will assume a classful address. So, `network 10.0.0.0` is assumed to be `10.0.0.0/8` this network command functions  like RIPs. you aren't actually telling the router to advertise the 10.0.0.0/8 network.
- You are telling it to activate EIGRP on interfaces with an IP address that falls in the 10.0.0.0/8 range, so any IP address that begins with 10. that includes G0/0 and G1/0 in activated on both interfaces.

- If you specify the mask, it looks like this,
![[Pasted image 20250403211933.png]]
- This command here, `network 172.16.1.0 0.0.0.15` activates EIGRP on the G2/0 interface.

*What is 0.0.0.15 ?*
> Isn't the subnet mask for a /28 prefix 255.255.255.240

**Yes, it is bur EIGRP uses a 'Wildcard mask' instead of a regular subnet mask.**

#### Wildcard Mask
![[Pasted image 20250403212328.png]]

![[Pasted image 20250403212353.png]]

- `0` In the wildcard mask = bits must match between the interface's IP address and the EIGRP `network` command
- `1` in wildcard mask = don't have to match

![[Pasted image 20250403214421.png]]

![[Pasted image 20250403214521.png]]

**Just remember, this network command only specify which interface or interfaces to activate EIGRP on R1 will then advertise the network prefix on that interface**

> 172.16.1.0/28  in this case.

###### Let's take a look at the show `ip protocols` commands when EIGRP is running.

![[Pasted image 20250403215049.png]]

- Routing protocol is EIGRP 1, 1 is being the AS number we configured of course.
- EIGRP uses interface bandwidth and delay by default, those are the `K1` and `K3` values that are set to 1 here.
- The bandwidth of the slowest link in the path plus the sum of the delay values of all the links in the path, are used to calculate the metric.
- The other K values k2, k4 and K5 are set to 0 and used to calculate metric, however that can be changed with configuration.
- Next up, is the router ID, in EIGRP and OSPF, the router has a unique router ID which identifies it within the AS. notice that the default on R1 is 172.16.1.14 
> why is that ? 
![[Pasted image 20250403215715.png]]

So, G2/0's 172.16.1.14 became the router ID. 
Note, that the router ID isn't actually an IP address, its just a 32-bit number formatted like a dotted-decimal IP address, and you can change it to any 32 bit number.

command : `eigrp router-id`
![[Pasted image 20250403215917.png]]

- Next is automatic summarization is disabled,
![[Pasted image 20250403220037.png]]
- EIGRP also performs ECMP load-balancing over a maximum path of 4 by default. 
![[Pasted image 20250403220126.png]]
- Routing for networks 10.0.0.0 and 172.16.1.0/28, these are the two network command we entered earlier.
- G2/0 is configured as a passive interface.
![[Pasted image 20250403220235.png]]
- There are two neighbours R2 and R3.
- EIGRP has two separate AD values, 90 for internal routes and 170 for external routes.
- Internal routes are normal EIGRP routes, but external routes are the routes from outside EIGRP which are then inserted into EIGRP  but that's a more advanced topic for the CCNP level.

### Let's look how EIGRP Look in routing table.

![[Pasted image 20250403220545.png]]

- First, notice EIGRP routes are indicated with the letter D, not E. Also check out the metric cost. these are much higher than you see in RIP and OSPF and this is a very small network in large network, these number can get much larger.
- that perhaps a downside of EIGRP, the metric are harder to understand.

