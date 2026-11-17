![[Pasted image 20250406002532.png]]

==> This to cover.
1. OSPF Network Types
2. OSPF neighbor/ adjacency requirements
3. OSPF LSA types

##### Loopback Interfaces
![[Pasted image 20250406002722.png]]

- A loopback interface is virtual interface in the router.
- It is always up/up (unless you manually shut it down)
- it is not dependent on a physical interface.
- So, it provides a consistent IP address that can be used to reach/identify the router.

 -> Lets say R1 has no loopback interface at the moment.
 -> R4 receives a packet destined for R1 at 10.0.13.1 the IP address of it's G1/0 interface. -> It might be forward it to R1 like this.
 ![[Pasted image 20250406003239.png]]
 -> what if R1 G1/0 interface goes down for some reason. If R4 receives a packet destined for R1 at 10.0.13.1 it will not be able to send the packet to R1.

=> How about if R1 has a loopback interface, 1.1.1.1 and is used to identify R1 instead of 10.0.13.1 ? Even if a physical interface fails, when R4 receives a packet destined for R1's loopback interface it will still be able to send the packet to R1 by another interface, that's why it's a good idea to configure a loopback interface on a router, It provides an interface with an IP address that is always up, and can consistently be used to identify and reach the router.


## OSPF Network Types.
- The OSPF 'network types' refers to the type of connection between OSPF neighbors (Ethernet etc.) and the type influences how OSPF behaves in some ways. The common type of connection in modern network is Ethernet

==> There are three main OSPF network types : 

**BROADCAST**

- enabled by default on Ethernet and FDDI (Fiber Distributed Data Interfaces) interface.

**Point to Point**

-  enabled by default on PPP (Point-to-point protocol) and HDLC (High-Level Data Link control) interface.
> You don't need to learn PPP and HDLC in depth for the current CCNA exam, but I will mention them briefly.

**Non-broadcast**

- enabled by default on Frame Relay and X.25 interfaces.

> Again, you need not to learn about these interfaces types in CCNA.


# Broadcast Network Type

![[Pasted image 20250406134923.png]]
> Enabled on Ethernet and FDDI interface by default.

- In the network diagram above, these are all Ethernet connections, remember 'G' means 'Gigabit Ethernet', and therefore these connections between the routers are all using the Broadcast network type. 

==> Now let's cover a few characteristics of the Broadcast network type.

- Routers dynamically discover neighbors by sending and listening for OSPF Hello messages using the multicast address 224.0.0.5, However, not all network types dynamically discovered neighbors like this. We won't cover this network type, but with the 'Non-broadcast' network type you must manually configure neighbors.

- A DR, Designated router, and BDR, backup Designated router, must be elected on each subnet. However in cases like G1/0 interface of R1,R3,R4 and R5 where there are no OSPF neighbors, there is just a DR, no BDR. 
- Router which aren't the DR or BDR for the subnet become a 'D R Other'

-> At the network above, Each subnet  needs a DR. These subnet are easy, there are no OSPF neighbors so each router becomes a DR for the subnet.
![[Pasted image 20250406142234.png]]

_How about the subnet 192.168.1.0/30 between R1 and R2? 
- We will see how to elect but lets say R2 is the DR. So R1 become BDR for the segment.
_How about the 192.168.2.0/29  subnet that R2, R3, R4 and R5 and R5 connected to ? 

>for ex : R5 might be the DR, R4 the BDR, and then R2 and R3 becomes the DROthers.
![[Pasted image 20250406143004.png]]

-> you might be wonder how these election works. and what the purpose of DR and BDR is well, lets get into it.

***==>So, here's how the DR and BDR are elected. There is an order of priority.***
- First up, router with the highest OSPF **interface priority** in the subnet become the DR for the subnet.
- However, all the interfaces have the same priority by default, so than the router compare their. OSPF **router IDs**. The router with the highest OSPF router ID wins. 
- 'First place' in the subnet becomes the DR in the subnet, 'second place' become the BDR.
- The default OSPF interface priority is 1 on all the interfaces. So as I said, by default the router with the highest router ID will become the DR for the segment.

## Summary
![[Pasted image 20250406143636.png]]

```
show ip ospf interface g0/0
```
![[Pasted image 20250406143738.png]]

- Notice the router ID on each router and the IP address of the loopback interface became the router ID.
![[Pasted image 20250406143849.png]]

- Notice state DR, and priority 1 default, R5 has the highest router id of the routers connected to 192.168.2.0/29 subnet so it became  the DR.
![[Pasted image 20250406144050.png]]

- here  DR, R5 itself and BDR R4 for the segment are listed , including their router ID and the interface IP address

--> Here is the same output for R2.
![[Pasted image 20250406144308.png]]

- The main difference I want to point out here is the state of DROther. Now, what if I want to make R2 the DR for the segment instead of R5? 

-->  Let's  see how to change the OSPF interface priority. the command to change the OSPF priority of an interface is.
```
ip ospf priority <priority>
```
![[Pasted image 20250406144712.png]]

--> I changed it to maximum, 255, on R2. As  a side note. *If you set the OSPF interface priority to 0, the router CANNOT be the DR/BDR for the subnet*

![[Pasted image 20250406144855.png]]

--> Lets check if R2 has became the DR for the segment.
![[Pasted image 20250406145015.png]]

-That's strange. R2's state is still DROther even though it has the highest priority. WHY ?

*That is because the DR/BDR election is 'non-preemptive'*

> Non-preemptive means that once the DR/BDR are selected they will keep their role until OSPF is reset, the interface fails/is shutdown, etc. 
![[Pasted image 20250406145459.png]]

-> although this is a bad idea to do in a live network, I'll go reset the OSPF process on R5 and let's see what happens

***==> So, I reset the OSPF  process on R5, and you can see all of its neighbors went DOWN.***
![[Pasted image 20250406151347.png]]
--> Then R2 and R4 returned to full state but not not R3, there is a important reason for that, we will see little bit later but for now.
![[Pasted image 20250406151408.png]]
--> I use the show IP OSPF NEIGHBOR command to view the neighbor state of R5 neighbor.
![[Pasted image 20250406151705.png]]
--> There are two important point about OSPF we can learn by analysing this section. 
- First R4 became the DR not R2. R2 became the BDR. --> When the DR goes down, the BDR becomes the new DR. Then an election happened for next BDR.
- R3 is a DROther, and is stable in the 2-way state. --> DROthers (R3 and R5 in this network) will only move to the FULL state with DR and BDR. The neighbor state other DROther will be 2-way.
![[Pasted image 20250406152707.png]]

- In the broadcast network type, router will only form a full OSPF adjacency with the DR and BDR of the segment.
- Therefore, routers will exchange LSAs with the DR and BDR. DROther will not exchange LSAs with each other.

> Remember, in the 2-way state router haven't shared LSAs with each other yet.

***==>  All routers will still have the same LSDB but this reduce the amount of LSAs flooding the network. Let's see an example.***

- If 6 routers are connected to the same segment and they all share LSAs with each other, it will end out like this. A whole lot of LSAs flooding and congesting the network. 
![[Pasted image 20250406153517.png]]

- How about if we use a DR and BDR?
![[Pasted image 20250406153600.png]]

- As you can see the number of LSAs flooding around the network is reduced. To be honest, on modern routers it's probably not a big deal in most cases, but it still does help reduce unnecessary network traffic.
- By the way, when routers  need to send message to the DR and BDR they use multicast address 224.0.0.6. This is different than the OSPF 'all routers' multicast address of 224.0.0.5.
![[Pasted image 20250406153944.png]]

- Remember I said the these first steps involve becoming neighbors? so, when routers reach this point in the process, they are OSPF neighbors. Connections between two DROthers stop at this point. Routers will continue on to exchange LSAs and from the Full OSPF adjacency with the DR and BDR. 
![[Pasted image 20250406154635.png]]
- So, to summarize, this means that the DR and BDR form a full adjacency with all routers in the subnet including the DROthers. and the DROthers will form a full adjacency only with the DR and BDR.
![[Pasted image 20250406154934.png]]

- We seen, SHOW IP INTERFACE BRIEF before. here I did on  R3, which is a DROther. 
![[Pasted image 20250406155405.png]]
- Notice the neighbor count on it's G0/0 interface. F indicates the number full adjacencies, and C indicates the total number of neighbors. So, R3 has 2 full adjacencies with R2 and R4, and 3 total neighbors R2, R4 and R5.

--> For more details, here is shown IP OSPF Interface G0/0 on R3. It gives the same information 
![[Pasted image 20250406155715.png]]



# Point-to-Point Network Type
![[Pasted image 20250406160244.png]]

> Notice that I changed the connection between R1 and R2 to a 'serial' connection. 

- This network type is enabled on serial interfaces using the PPP or HDLC encapsulations by default, PPP and HDLC are both Layer 2 encapsulations, similar to Ethernet, except they are used on serial connections.
- Same as Broadcast network type, routers dynamically discover neighbors by sending/listening for OSPF Hello message using multicast address. 224.0.0.5. However, here's a difference A DR and BDR are NOT elected. Why is that?
- As the network type name implies, these encapsulations are use for 'point-to-point' connections between two routers. There there is no point in electing a DR and BDR. The two routers. will form a full adjacency with each other, without the need to elect a DR and BDR.

*here is the brief overview of serial connections. very brief because serial connection are an old technology which isn't very command anymore. They still exits but Ethernet is much more dominant, In fact serial connection  have been remove from the exam topic except for the OSPF point to point network type.*
![[Pasted image 20250406161459.png]]
> this photo shows some serial interfaces and cables notice that both the port and cables are different the Ethernet cable.

-->  To explain serial connections, I'll show you how I configured R1's S2/0 interface. 

![[Pasted image 20250406162010.png]]
- First up, One side of serial connection functions as DCE, which stands for Data communications Equipment. The other side functions as DTE , which stands for Data Terminal Equipment.
- Why is this significant? well, on serial connections, the DCE side needs to specify the clock rate. which is the speed of the connection.
- In this case R1 has the DCE side of the cable. and therefore need to tell R2 what speed the connection will operate at. the command is `clock rate` and then you can see some standard values that can be used. all of these are in bits per seconds by the way.
![[Pasted image 20250406162220.png]]

- I configured a clock rate of 64000 bits (aka 64kb) per seconds 
![[Pasted image 20250406162811.png]]

- Let's check the interface with SHOW INTERFACE command.
```
show interface s2/0
```
![[Pasted image 20250406162936.png]]
- Notice that the encapsulation is HDLC. On cisco router default encapsulation on a serial interface is HDLC.
-  Once again, this is a layer 2 encapsulation, like ethernet. except its used on serial connection.

Here is the structure of an HDLC from.
![[Pasted image 20250406163253.png]]

- One thing to notice is that there is no MAC address field. So, MAC address actually aren't used. I mentioned the PPP encapsulation too. Lets see how to configure the router to use that encapsulation instead. 
- Simply use the ENCAPSULATION PPP command on the interface.
![[Pasted image 20250406163646.png]]
- Note that If you change the encapsulation, it must match on both ends  or the interface will go down.

==> Here is the configuration on R1
![[Pasted image 20250406163846.png]]
- the serial restart delay command is there by default. 

==> Here is the configuration on R2
![[Pasted image 20250406163937.png]]
- notice there is no clock rate command because it is the DTE end.

> Now how can you tell which side is DTE and Which side is DCE?

 Command to see it is 'SHOW CONTROLLERS' followed by the interface. 
 ![[Pasted image 20250406164225.png]]
![[Pasted image 20250406164245.png]]

## Summary 
![[Pasted image 20250406164342.png]]

> let's back to POINT-TO-POINT network type
![[Pasted image 20250406164421.png]]
- Notice that  R2 has a full adjacency with R1, but instead of DR, BDR or DROther a dash is displayed. This is because the point-to-point network types doesn't use DRs and BDRs, as I mentioned before.

**==> You can manually configure the OSPF network type an interface uses.**
- The command is IP OSPF NETWORK, followed by the network type. you might notice one more that I haven't mentioned, that is 'point-to-multipoint' network type.
- It's more of a sub-type. you don't need to learn about this for CCNA

**==> So, why would you change the OSPF network type?**
- for example, if two routers are connected with a ethernet link, there is no need to for a DR/BDR. You can configure the point-to-point network type in this case.
![[Pasted image 20250406165345.png]]

- Note : Not all network types works on all link types (for example, a serial link can not be use the broadcast network type.
![[Pasted image 20250406165532.png]]

# OSPF Network Requirements

![[Pasted image 20250406185734.png]]

##### 1. Area Number must match.
![[Pasted image 20250406185807.png]]

R1 and R2 OSPF config. both in different area.
![[Pasted image 20250406185917.png]]

--> If we check `ip ospf neighbor` on both devices, they have no OSPF neighbors
![[Pasted image 20250406190025.png]]

--> Let's fix the issue. I changed the network command of R2 to use area 0
![[Pasted image 20250406190111.png]]

**--> Now as you can  see they have successfully became OSPF neighbors**
![[Pasted image 20250406190156.png]]

##### 2. The interfaces must be in the same subnet to become OSPF neighbors.

![[Pasted image 20250406190434.png]]

R1 and R2 OSPF config. both are in different subnet.
![[Pasted image 20250406190602.png]]

--> If we check `ip ospf neighbor` on both devices, they have no OSPF neighbors
![[Pasted image 20250406190025.png]]

--> Once again let's configure R2's interface in the same subnet as R1
![[Pasted image 20250406190832.png]]

**-->Now as you can  see they have successfully became OSPF neighbors**
![[Pasted image 20250406190913.png]]

##### 3. The OSPF process must not be shutdown.
> You can actually 'shutdown' the OSPF on the router like an interface. This disables OSPF operations, without removing the OSPF configurations.

--> From OSPF configuration mode on R2, I used the SHUTDOWN command.
- A message is displayed indicating that its neighbor R1 has gone from FULL to DOWN, and no neighbors are displayed.
![[Pasted image 20250406191413.png]]

**--> Now again. Lets Enable OSPF again with NO SHUTDOWN command**
- A message is displayed that the neighbor is back up, and you can see it in show ip OSPF neighbors
![[Pasted image 20250406191558.png]]

> This one won't be a problem unless you manually shutdown the OSPF process, so usually it's  not a problem. 

##### 4. The OSPF router ID must be unique.
![[Pasted image 20250406191839.png]]

--> We haven't configured the router IDs of R1 and R2, and we also haven't configured any loopback interface, so each router has selected it G0/0 IP address as its router ID, 192.168.1.1 for R1 and 192.168.1.2 for R2. So I configured 192.168.1.1 as R2's router ID, the same as R1's.

--> As I have shown you before, you need to reload the router  or use clear IP OSPF process for the new router ID to take effect. So  I used CLEAR IP OSPF PPROCESS for the new router ID to take effect.

![[Pasted image 20250406192400.png]]

--> But then, instead of neighbour coming back up, this message is displayed. OSPF detected duplicate router ID 192.168.1.1 from 192.168.1.1 on interface G0/0, and the neighbor stays down.

**==> Lets fix this, I removed the manually configured router ID with NO ROUTER-ID**

![[Pasted image 20250406192708.png]]

> Note that you actually have to specify the router ID when removing router ID. No router ID has the same effect as NO ROUTER-ID 192.168.1.1, but both remove the command.

--> This time, without having to reset the OSPF process R2's router ID returns to 192.168.1.2 and the neighbor comes up again.
![[Pasted image 20250406193035.png]]

==> Why didn't I have to use CLEAR IP OSPF PROCESS ? 
- I actually didn't expect this, but I realize that's because R2 had no other OSPF neighbors. at the time so the router was free to change R2's router ID without worrying about it effecting other neighbors. 

#####  5. The Hello and Dead Timers must match.
![[Pasted image 20250406193504.png]]

- Here's how to change the Hello timer. 
```
ip ospf hello-interval <interval>
```
--> I set it to 5.

- Here's how to change the Dead timer.
```
ip ospf dead-interval <interval>
```
--> I set it to 20 

![[Pasted image 20250406193720.png]]

===> As you can see the neighbors then goes down. Note that I changed both values, the hello timer and Dead timer, but even if you change one the neighbors will still go down,

**--> Let's fix this, I use NO IP OSPF HELLO-INTERVAL and NO IP OSPF DEAD-INTERVAL**
- to return the timer to default values.
![[Pasted image 20250406194152.png]]

> Notice the method I used to return them to their defaults, it's same as for the router ID. I didn't have to specify `no ip ospf hello-interval 5` or `no ip ospf dead-interval 20`  to remove the commands.

*So, remember to check the Hello and Dead intervals if you're having the trouble with OSPF adjacencies*

##### 6. Authentication Setting Must Match.

--> You can configure an OSPF password, and then the router only become neighbors with routers that have a matching OSPF password.

--> lets take a look. 

- The OSPF password is configured directly on the interface too. I used IP OSPF AUTHENTICATION-KEY,  followed by a password.
```
ip ospf authentication-key
```
- Notice this doesn't actually enable OSPF authentication on the interface, the password is configured but need to enable OSPF authentication. So I used IP OSPF AUTHENTICATION
```
ip ospf authetication
```
> then the neighbor goes down, because R1 isn't providing R2 a matching password. actually I haven't configured the OSPF authentication yet. so it's not providing R2 with any password so.

**==> To fix this, we could either configure the same authentication settings on R1, or remove the authentication from R2**

-> I remove the authentication from R2, and now the neighbor is back up again.
![[Pasted image 20250406195556.png]]


***There are two more things to mention but these are two special requirement***
![[Pasted image 20250406201252.png]]

##### 7. IP MTU Settings must match.
 --> The IP MTU is the maximum size of an IP packet that will be sent out of the interface. The default is usually 1500 bytes, but it can be configured.

- Note that this requirement and next one are special, even if the settings don't match, the neighbors can become OSPF neighbors, but OSPF won't function properly.

==> Let's see...
![[Pasted image 20250406200018.png]]

- You can configure the IP MTU of an interface with this command:  IP MTU followed by MTU in bytes.
- I change it to 1400 on R2's G0/0, So it doesn't match with the default 1500 on R1's G0/0.
- I waited a minute and then checked neighbor table. R1 and R2 actually remains neighbors.
![[Pasted image 20250406200259.png]]

- Then I reset the OSPF process on R2. I waited a minute. but no message indicating that the neighbors had reached the full state. I check the neighbor table again. and it was stuck in the EXSTART state.
- After waiting a few minutes A few messages were displayed, and these kept on repeating by the way.

**==> So clearly, OSPF isn't functioning properly. I used NO IP MTU to return to the default value.**
![[Pasted image 20250406200646.png]]

- and finally R1 and R2 reached to the full state.

##### 8. OSPF Network Type Must Match

-  I configured a loopback interface on R2 with an IP address, and advertise it to R1. Then I changed the network type on R2's G0/0 to point-to-point 
> R1 is still using the broadcast network type.
![[Pasted image 20250406201622.png]]

- A Message is displayed saying the neighbor went down, but then it went right back to full state. R1 even displays as full in show ip OSPF neighbors, So, what's the problem? 
--> Let's see on R1

- R2 appears in the neighbors table in full state, so you might think OSPF is working fine. but look at the routing table. R2's loopback address should be in R1's routing table, but it's not, This is what will happen when the OSPF network doesn't match.
- It can be tricky to troubleshoot because the neighbor state is full. So it can seem like every this working fine.
- Make sure to watch out for that when troubleshooting OSPF.

> There is of course more we can get into but for CCNA this is enough.


# LSA Types 
![[Pasted image 20250406202855.png]]

->  these aren't specifically mentioned in the exam topic list. so let's just have  a brief overview of some basic LSA types.

-  R4 is having a default route to the internet and I use DEFAULT-INFORMATION ORIGINATE to make it advertise it to the other routers. So, Let's talk about LSAs.
- All routers In the same OSPF area share the same LSDB,  
- There are 11 types of LSAs but there are only 3 you should know for the CCNA
![[Pasted image 20250406203409.png]]

![[Pasted image 20250406203522.png]]

Here's a look at the LSDB on R1, using the command show IP OSPF DATABASE. Note that it doesn't actually matter which router I use the command on, the output will be the same. because all routers in the area have the same LSDB. 

![[Pasted image 20250406203708.png]]
 
> Notice that each router in the area have the same LSDB. Notice that each router has generated a Type 1 Router LSA identifying itself.

- This view doesn't show the content of each LSA, but each of these router LSAs contains information about the networks each router is connected to.
- Notice that there is only one Type of 2 Network LSA, generated by R4 for the 192.168.2.0/29.
- Even though, R1,R2 R3 and R5 are DRs on their G1/0 interfaces, no other routers are connected to the interface so no Type 2 LSA is generated. 
- Finally Notice that a  type 5 AS-External LSA is generated by R4 this is sharing  its default route to internet with other routers.

