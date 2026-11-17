  ![[Pasted image 20250405141027.png]]
*=> Topic for the Part*
- OSPF metric cost
- Become OSPF neighbours
- More OSPF configuration


# OSPF Metric (Cost)
- OSPF metric is called cost.
- It is automatically calculated based on the bandwidth (speed) of the interface.
> You can also manually configure the cost of each interface, will see later.
- It is calculated by dividing a reference bandwidth value by the interface's bandwidth.
- The default reference bandwidth is 100 mbps.

-> for example : a regular Ethernet interface with a speed of 10 mbps has  a OSPF cost of 10, because 100 divided by 10 equals 10.
![[Pasted image 20250405141816.png]]
- All values less than 1 will be converted to 1.
![[Pasted image 20250405141843.png]]

-> let's see that in CLI

> here is the same network topology as before.
![[Pasted image 20250405141920.png]]

- Let's checkout the cost of R3's F2/0 interface.
```
show ip ospf interface f2/0
```
![[Pasted image 20250405142218.png]]

- As I said, the default cost of a Fastethernet interface is 1.

- Now, Lets check out the cost of R3's G0/0 interface.
```
show ip ospf interface g0/0
```
![[Pasted image 20250405142447.png]]
>  As, you can see the cost here is 1 as well.

***=> Clearly the default situation is not ideal. Fortunately, you can change this.***

- You can, you should, change the reference bandwidth with this command, from OSPF configuration mode. 
```
auto-cost reference-bandwidth ? 
```
![[Pasted image 20250405142820.png]]
![[Pasted image 20250405142918.png]]

- The 100000 mbps I configured as the reference bandwidth is equal to a 100 gigabit per second, which is 100 times faster than the fastest interfaces in this network, regular gigabit ethernet.

- Finally, notice this message that is displayed when you configure the reference bandwidth. 
![[Pasted image 20250405143309.png]]
- So, to provide a consistent cost of each interface bandwidth across the network, you  should configure the same reference bandwidth on all OSPF routers in the network.

- The OSPF cost to a destination is the total cost of the outgoing/ exit interfaces. it is just like spanning tree cost.

-> For example.
![[Pasted image 20250405143751.png]]

![[Pasted image 20250405143804.png]]


=> This is the routing table before changing the default reference bandwidth  on all of the routers,  So they all have the  r.b. of 100mbps

![[Pasted image 20250405144053.png]]
> Notice, Even though the connection between R3 and R4 is a slower Fastethernet connection, it has the same cost of 1 as the gigabit Ethernet interfaces.


=> This is the routing table after changing the default reference bandwidth  on all of the routers,  So they all have the  r.b. of 100000mbps
![[Pasted image 20250405144140.png]]
> Now, R1 only inserts one route to 192.168.4.0 in the routing table, and the cost is 300

-> Notice the cost to 2.2.2.2 is 101, like also calculated before for loopback interface.


## Manual OSPF Cost Configuration of an Interface
- The command is `ip ospf cost`.
![[Pasted image 20250405145039.png]]

- You configure this directly on the interface and this cost will take priority over the auto-calculated cost.
- For example, I configured the cost of R1's G0/0 interface as 10,000, and now you can see the cost is 10,000 instead of 100.
![[Pasted image 20250405145411.png]]

***==> One more options to change the OSPF cost of an interface is to change the bandwidth of the interface with the bandwidth command.***

***===> As you know  the formula to calculate the OSPF cost is = reference bandwidth / interface bandwidth***

![[Pasted image 20250405151207.png]]

***==> To change the speed at which the interface operates, use the speed command***

- if you change the `bandwidth` of gigabit ethernet interface to 100mbps, it will still operate at 1 gigabits per second. however for the purpose of OSPF's cost calculation, the bandwidth is 100 mbps.

![[Pasted image 20250405151606.png]]

-> still, if you wan to change use the, command `bandwidth` to change.

-> Notice :this is different than reference bandwidth which is entered in mbps, The interface bandwidth command is entered in kbps.

### Summary 
![[Pasted image 20250405151936.png]]

-> way to check the OSPF cost of all interfaces.

![[Pasted image 20250405152017.png]]


# OSPF Neighbours.

***==> Making sure that routers successfully become OSPF neighbours is the main task in configuring and troubleshooting OSPF***

=> Once router becomes neighbours, they automatically do the work of sharing network information, calculating routes etc.

=> So, you just have to make sure that OSPF is activated on the correct interfaces, and that the proper conditions are met to allow the routers to become neighbours.

=> Of course, there are more advanced OSPF configuration you can do, but for the basic OSPF operations they aren't necessary.

=> However, if router can't become OSPF neighbours, OSPF can't operate at all, so this is very important.

**So, how do routers become OSPF neighbours ?**

 ![[Pasted image 20250405154245.png]]
 - By exchanging hello messages they check that they are compatible to become OSPF neighbours, and negotiate their neighbour relationship
 - By the way, the default hello timer is 10 seconds on an Ethernet connection.

![[Pasted image 20250405154510.png]]
> RIP multicast address is 224.0.0.9 for EIGPR is 224.0.0.10

-> OSPF messages are encapsulated in an IP header, with a value to 89 in the protocol field.


- Okay, for OSPF routers to become neighbours they have to go through various neighbour states.
## Let's have  a basic overview of each of the neighbour states.
![[Pasted image 20250405225550.png]]

![[Pasted image 20250405155246.png]]
> let's assume OSPF is already activated on R2's G0/0 interface,

1.  OSPF is activated on R1's G0/0 interface. 
2. It sends an OSPF hello message to 224.0.0.5.
3.  There are more fields in the hello message but two important ones are R1's router ID and neighbours router ID. 
![[Pasted image 20250405215430.png]]
4. However R1 doesn't know about R2 yet, so the neighbour router ID field is 0.0.0.0. 
5. R1 doesn't know about any OSPF neighbours yet, so the current neighbour state is **Down.**
> This is the first OSPF neighbour state. DOWN

6. When R2 receives the Hello packet, it will add entry for R1 to its OSPF neighbour table.
7. In R2's neighbour table, the relationship with R1 is now in **Init  state**.
> This is the second OSPF neighbour state.
![[Pasted image 20250405220535.png]]
8. Note that R1 still don't know about R2, so it will have no entries in its OSPF neighbour table.
9. Basically, the initial state means that a hello packet was received. but R2's own router ID is not in the Hello packet. 
10. R2's router ID is 2.2.2.2, but the neighbour router ID in R1's Hello packet is 0.0.0.0

11. The Next state is **2-way state**, R2 will send a Hello packet containing the RID of both the routers.
![[Pasted image 20250405220847.png]]

12. Than R1 will insert into OSPF neighbour table in the 2-way state, Then R1 will send another Hello message,  this time containing R2's RID

> Now both Routers are in 2-way State.

*The 2-way state means the Router has received a Hello packet with its own RID in it.*

***==> If both routers each 2-way state, it means that all of the condition have been met for them to become OSPF neighbours.  They are now ready to share LSAs to build a command LSDB***

13. On the other hand, if they failed to reach this 2-way state, you know that you have to troubleshoot and find what's stopping them from reaching it.
![[Pasted image 20250405221648.png]]

=> At this point, the routers are already OSPF neighbours. Over the next few neighbour state they will share LSAs and form a Full OSPF adjacency. Lets go to next neighbour state.

14. After the 2-way state, the two routers will now prepare to exchange information about their LSDB.
15. Before that they have to choose which one will start the exchange. So, they will decide which one will be the Master router and which one will be the Slave. These are different than DR and BDR.
16. This master/slave relationship is only needed for this initial exchange of LSDB information
17. They will decide which will be the master and which will be the slave in the **Exstart state**.
18. The router with the higher RID will become the master and initiate the exchange. The router with the lower RID will become the slave. So in this cast R2 will be the master and R1 will be the slave.
![[Pasted image 20250405222558.png]]
19. Basically, the Exstart state is just to prepare for the next state.
![[Pasted image 20250405222845.png]]

20. In the next state, the **Exchange state**, the routers exchange DBDs which contain a list of the LSAs in their LSDB.
21. These DBDs do not include detailed information about the LSAs, just basic information. telling the neighbour what LSAs they have.
22. Basically the router are telling each other, I have these LSAs, but they aren't actually sending the LSAs yet.
![[Pasted image 20250405223242.png]]

23. The router compares the information in the DBD they received to the information in their own LSDB to determine which LSAs they must receive from their neighbour.

> after Exchanging DBDs, they move to the next state.

24. The next state is the **Loading state**. In the loading state, routers send *Link State Request(LSR)* message to request that their neighbours send them the LSAs they don't have.
25. In the Exchange state they exchange DBD packets, so they know which LSAs their neighbours are holding, So these LSRs are used to request any missing LSAs to make sure each router has the same LSAs.
26. Here  I will just show one side of exchange, but R2 will also send R1 LSRs for any missing LSAs.
27. then the LSAs themselves are sent in *Link state Update (LSU)* Message. R2 sends R1 the requested LSAs in an LSU like this diagram below.(R1 will also do the same for R2 )
28. Finally the router send LSAck message, another kind of OSPF message, to acknowledge that they received the LSAs.
![[Pasted image 20250405224700.png]]

29. Now the loading state is complete. and the routers have the same LSDB.
30. now, we reached the OSPF final state. in the  **Full state**, the router have a full OSPF adjacency and identical LSDBs, but that doesn't means things are complete.
31. They continue to send and listen for Hello packets, once every 10 seconds by default. to maintain the neighbours adjacency.
32. To maintain the adjacency  another time is called the 'Dead timer ' is used. Every time a Hello packet is received, the 'Dead timer', which is 40 seconds by default, is reset. However if the Dead timer counts down to 0 and no Hello messages is received, the neighbour is removed.
33. If the neighbour remain up, the router will continue to share LSAs as the network changes to make sure each router has a complete and identical map of the network. 

> this is the main advantage of dynamic routing protocols, the routers automatically react to changes in the network and add, remove or change as necessary.


### Summary

![[Pasted image 20250405230104.png]]

![[Pasted image 20250405230132.png]]

## OSPF commands

![[Pasted image 20250405235133.png]]

```
show ip ospf neighbor
```
![[Pasted image 20250405235230.png]]
- Note the full state with both neighbor R2 and R3. also, both R2 and R3 are DR's.
- Also, notice the dead time this counts down from 40, but resets as soon as R1 receives a Hello packet from the neighbor.

```
show ip osfp interface g0/0
```
![[Pasted image 20250405235553.png]]
-  you can see the `hello 10` and `Dead 40`, Hello due to in 7 seconds means that R1 will send a Hello message out of this interface in 7 seconds. as it does once ever 10 seconds.
![[Pasted image 20250405235748.png]]
- R1 has only one neighbor connected to it's G0/0 interface, that's R2.
![[Pasted image 20250405235948.png]]
- Below that, adjacent neighbor 2.2.2.2, designated router.

## OSPF Configuration
![[Pasted image 20250406000150.png]]

***==> First off, do you remember the purpose of the `network` command ? Its same for RIP, EIGRP and OSPF. It simply tell the router which interfaces to activate the routing protocol on.***

==> Well, you can actually enable OSPF directly on an interface, without using the NETWORK command. 

For ex : lets assume that R1 has no OSPF configuration on it yet. Here's how to enable OSPF on the interfaces.
![[Pasted image 20250406000647.png]]
- You can activate OSPF directly on an interface with this command.
```
ip ospf <process-id> area <area>
```
> Note that this is done from interface configuration mode.

Now, OSPF is enabled on those interfaces and I haven't entered OSPF configuration mode.

***==> Next, another method to configure passive-interfaces***
![[Pasted image 20250406001000.png]]
```
passive-interface default
no passive-interface <interface-id>
```

Can you see the difference, You can configure all of the router interfaces to be OSPF passive interfaces by default with this command, Then you can use NO PASSIVE-INTERFACE to remove that only from specific interfaces.

- Depending on the number of passive interface you need to configure, this method might be faster, or the normal method is faster. either way the effect is same.

==>  If you configure OSPF directly on the interfaces, you'll see slightly different output from  show IP PROTOCOL.
![[Pasted image 20250406001439.png]]

- The *Routing for Networks* section is empty, and instead the interfaces you activated OSPF is displayed  here, 'router on interfaces configured explicitly'

The rest of the output is same however.

### Summary
![[Pasted image 20250406001813.png]]

![[Pasted image 20250406001822.png]]

![[Pasted image 20250406001845.png]]

### Question
![[Pasted image 20250406002351.png]]
