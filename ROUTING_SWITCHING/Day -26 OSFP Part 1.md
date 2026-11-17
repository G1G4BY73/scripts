![[Pasted image 20250404143326.png]]

=> OSPF is Link state dynamic routing protocol, works quite differently than the distance vector routing protocols RIP and  EIGRP.

=> Distance vector use 'routing by rumor'  in which each router share information about the routes they know and their metric cost to reach each destination. However the router don't have the complete map of the network. they just use the information neighbouring routers tell them to determine the best route to each destination.

###  Link State Routing protocol
![[Pasted image 20250404144001.png]]

# OSPF 
=> Open shortest path First

- OSPF uses the Shortest Path First algorithm of Dutch computer scientist Edsger Dijkstra (aka Dijkstra's algorithm)
- There are Three versions of OSPF.
	- OSPFv1 (1989): OLD, not in use anymore
	- OSPFv2 (1998): Used for IPv4
	- OSPFv3 (2008): Used for IPv6 (can be used for IPv4, but usually V2 is used)
- Routers store information about the network in **LSAs** (Link State Advertisements), which are organized in a structure called the **LSDB** (Link State Database.)
- Routers will floods LSAs until all routers in OSPF area develop the same map of the network (LSDB).

Lets say this four routers are running OSPF.
![[Pasted image 20250404163340.png]]

- All of these routers are OSPF neighbours, they have the same like state database and the network is stable. Then OSPF is enabled on R4's G1/0 interface.
- So, R4 needs to tell the other routers about this new  network segment.
![[Pasted image 20250404163615.png]]
- So, R4 creates an LSA to tell its neighbours about the network on G1/0.
- An OSPF LSA contains more information than this, and we'll look deeper later, but some of the basic information in an LSA is the RID, the router ID.
- For this demonstration, R4 router ID is 4.4.4.4, None of this physical interfaces have an IP address of 4.4.4.4, so either R4 has a loopback interface with the IP address 4.4.4.4 or the router ID was manually configured.
- The network on the G1/0 interface is, of course included in the LSA, since that's the main purpose of LSA. R4's cost is also included.
![[Pasted image 20250404170243.png]]

- The LSA is than flooded throughout the network until all routers receive a  copy.
- This results in all routers sharing the same LSDB.
- This LSDB is like this containing LSAs for all of the different links in the network.
![[Pasted image 20250404170513.png]]
- Now that OSPF has been activated on R4's G1/0 interface. that net LSA id added to the LSDB.
![[Pasted image 20250404170606.png]]
- This LSDB is identical in all OSPF area
- Each router then uses the SPF algorithm, Dijkstra's algorithm, to calculate its best route to 192.168.4.0/24. Remember, each of these router has a complete map of network.
- So, for example you and I can looking at this diagram can see that R2's best route to 192.168.4.0/24 it this route via G1/0 interface.
![[Pasted image 20250404173836.png]]
- Well R2 is basically looking at the same diagram, so it can calculate that sending traffic via G1/0 is the best route. it's not looking at visual diagram like we are, of course, but in effect its the same thing.
- Finally note that Each LSA has an aging timer(30 min by default). The LSA will be flooded again after the time expires.

##### OSPF Process
![[Pasted image 20250404180424.png]]

Lets move on to another fundamental part of OSPF, which isn't part of RIP and EIGRP.

# OSPF Areas
- OSPF uses areas to divide up the network.
- Small network can be single-area with any negative effects on network performance.
- For example this network with 4 routers are small network.
![[Pasted image 20250404180713.png]]
- In larger network, a single-area design can have negative effects.

![[Pasted image 20250404181259.png]]

=> Lets see a example here...
![[Pasted image 20250404192514.png]]

- Here also it is possible to make one large, single area network, All interfaces and all routers are assigned to area 0, also know as the backbone area.
- You will see soon that area 0 is of special importance in OSPF, and it's called the `backbone` area.
- Now instead of one large area, lets see how the network can be divided into separate areas.
![[Pasted image 20250404193005.png]]

=> Now, there are some rules and terminology you should about OSPF areas, 

###### What is an Area ? 
- An area is a set of routers and links that share the same LSDB.
- Each of these areas maintains a unique LSDB. 
- Next, the backbone area ( area 0 ) is an area, a special area, that all other areas must connect to.
- Notice is network diagram area 1, area 2 and area 3 all connect to area 0, the backbone area.
> This kind of network design, for example, is not allowed in OSPF.
![[Pasted image 20250404193456.png]]
Notice that area 1 is not connected to area 0, the backbone area. it is only connected to area 2, this is not allowed in OSPF.

- Next up, routers with all interfaces in the same OSPF area are called `internal routers`.  So now in our diagram below highlighted are the internal routers
![[Pasted image 20250404193726.png]]

- Routers with interfaces in multiple OSPF areas are called area border routers (ABRs). because they are the borders between different OSPF areas.
![[Pasted image 20250404194013.png]]

![[Pasted image 20250404194042.png]]

- Routers connected to the backbone area (area 0) are called backbone routers. This includes area border routers, by the way.
![[Pasted image 20250404194309.png]]

- An intra-area route is a route to a destination inside the same OSPF area. For example, from a router in area 1 to a destination that is also in area 1. 

> For example, this router  learns a route to this subnet, it is connected to an intra-area route, because the destination is in the same area as the router.
![[Pasted image 20250404194749.png]]

- An inter-area route is a route to a destination in a different OSPF area. for ex: if a router in area 1 learns a route to a destination in area 2, that is an interarea route.
![[Pasted image 20250404194920.png]]
Here the router and the destination is in two different OSPF areas.

### OSPF Areas Summary
![[Pasted image 20250404195009.png]]

#### Some rules related to OSPF areas
1. OSPF areas should be contiguous. it means that each individual area should be connected, not divided up

> Not contiguous  looks like below example. Area 1 is here non contiguous. instead of all being connected half of area 1 is here and half of area 1 is here. this kind of design is now allowed in OSPF and will cause problems.
![[Pasted image 20250404200601.png]]
> So, instead of having area 1 split up and non-contiguous like this, you should make this section on the right a separate area. area 3

2. All OSPF areas must have at least one ABR connected to the backbone area.
3. OSPF interfaces in the same subnet must be in the same area.  If they are not in the same area, they won't become OSPF neighbours and won't exchange information  about the network they know about.

> Let's see a example.
![[Pasted image 20250404201402.png]]
> In this example, these three routers all have an interface in area 0,  in 192.168.1.0/29 subnet. this router in area 1 also has an interface in the 192.168.1.0/29 subnet, but the interface is in area 1, not area 0.
> Even though all four interfaces are in the same subnet and OSPF is enabled on them, the area 1 router will not become OSPF neighbours with the other.

![[Pasted image 20250404201743.png]]
> This time, the area 1 ABR's interface in the 192.168.1.0/29 subnet is correctly configured in area 0. so all four routers will become OSPF routers.

#### Summary 
![[Pasted image 20250404201926.png]]


## Some basic OSPF configuration.
> Lets use the same topology we used for the RIP and EIGRP

![[Pasted image 20250404202049.png]]

> For the CCNA we only have to configure single area OSPF. so all of these router interfaces are in OSPF area 0.

>R2, R3 and R4 are already configured, so let's start configuring OSPF on R1.

- Here is the basic configuration of OSPF
![[Pasted image 20250404202959.png]]

- First up, to enter OSPF configuration mode use the command `router OSPF`, followed by a process ID.
- A router can run multiple OSPF processes at once, and this ID is used in the router to identify each of them. Typically we'll just use a single OSPF process, so don't worry about this number. I choose to use 1.
- In EIGRP configuration we used command `router eigrp`, followed by an AS number for EIGRP routers to become neighbours, their AS number has to match, however the OSPF process ID id different.
- The OSPF process ID is locally significant, routers with different process IDs can become OSPF neighbours, Usually  I just used process ID 1, but you could for example process ID 1 on R1 and 2 on R2, and they would still become OSPF neighbours and exchange LSAs.
> Note that, This process ID is totally unrelated to area.

- Next, I attempted to use the `network` command just like EIGPR. with a network subnet and their wild card. but router says incomplete command. That because OSPF network command requires you to specify the area. so I activated OSPF on all of the interfaces in area 0.
> Here also like EIGRP we use wildcard mask in OSPF.

> for the CCNA, you only need to configure single-area OSPF (area 0)

> for single area OSPF, it's actually possible to use any number, but it's considered best practice to use area 0.

![[Pasted image 20250404204539.png]]

*So, just remember that the network command is used to tell the router which interfaces is to active OSPF on. It doesn't tell routers 'advertise these networks'*

- **Passive Interface** : you already know this command from RIP and EIGRP, it's totally the same for OSPF. The `passive-interface` command will tell router to stop sending OSPF 'hello' messages out of the interface.

- However, the router will continue to send the LSAs informing it's neighbours about the subnet configured on the interface.
- So, although R1 won't send hellos out of G2/0 interface and try to find OSPF neighbours, it will still tell its other neighbours about the 172.16.1.0/28 network. as it is configured with network command.
- you should always use this command on interfaces which don't have any OSPF neighbours. it is waste to continuously send 'hello' messages out of an interface with no other router connected to it. 

> So, that's the passive-interface command, it's basically the same as for RIP and EIGRP.

- Next, Lets see how to advertise a default route into OSPF, just like we seen for RIP. so lets add a internet connection to R1.
![[Pasted image 20250404211554.png]]

- Then configured a default route on R1, and next hop is the ISP's IP address of 203.0.113.2
![[Pasted image 20250404211737.png]]
- here is the information in the routing table of R1
![[Pasted image 20250404211815.png]]

- Just like in RIP, the command to advertise the default route into OSPF is `default-information originate`. In OSPF this will cause the router to create a new LSA and Flood it.
![[Pasted image 20250404211953.png]]

Now, if we check R2's routing table. you can see it added the default route via R1 to its route table R3 and R4 will do the same.

###### SHOW IP PROTOCOL
-> Now lets a look at the `show ip protocols` command from an OSPF prospective, and will check out some other command as well.
![[Pasted image 20250405124602.png]]

- Up top, it says 'routing protocol is OSPF 1'. 1 this the process ID we configured earlier.
- OSPF uses the router ID, and the router ID is determined in exactly the same way as in EIGRP.
![[Pasted image 20250405124800.png]]
- How to manually configure router ID.
![[Pasted image 20250405124845.png]]
> it is  little different from EIGRP, in EIGRP the command is `eigrp router-id` but in OSPF its just `router-id`

> ! Error : So, currently, the router  ID is still 172.16.1.14, to make 1.1.1.1 take effect we have to either reload the router or use that command to clear OSPF process and reset it

![[Pasted image 20250405125202.png]]

> This basically resets OSPF on the router. This is a bad idea in a real network and router will lose all of its OSPF routes. for short time and won't be able to forward traffic to those destinations.

![[Pasted image 20250405125343.png]]

> When you see this  no in square brackets after a command, it means no is the default choice.

![[Pasted image 20250405125532.png]]

![[Pasted image 20250405132714.png]]

 ![[Pasted image 20250405134214.png]]
- These are three different types of OSPF areas, it's not necessary to know about the different  types for the CCNA, just wanted to point that you can see the number of areas this router is in, just one because this is single-area OSPF.

![[Pasted image 20250405134436.png]]

- Unlike EIGRP, OSPF doesn't support unequal cost load balancing, but it does support ECMP load-balancing over a maximum of 4 paths by default.
- To change the maximum number of path, use the maximum-paths command just like in RIP and EIGRP.
![[Pasted image 20250405134654.png]]
- The 'routing for networks' section shows the network command we used.
![[Pasted image 20250405134759.png]]
- This determines which interfaces OSPF will be activated on, it doesn't tell OSPF to flood LSA's for these specific networks.
- Here's the passive interface we configured.
![[Pasted image 20250405134938.png]]
- and, here R1's neighbour.
![[Pasted image 20250405135026.png]]

- Notice the router IDs, I configured these loopback interfaces on R2, R3 and R4 and their IP address became the router IDs.
- Finally down here, OSPF's As is displayed the default is 110 as  you know.

- If you want change it, the command is the same as for RIP and EIGRP. from OSPF configuration mode, just use the command distance. For ex. I changed it to 85, so OSPF are preferred over EIGRP route on this router.
![[Pasted image 20250405135414.png]]
