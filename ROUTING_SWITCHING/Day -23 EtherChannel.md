EtherChannel allows you to group multiple physical interfaces into a group which operates as a single logical interface, so they behave as if they are a single interface.


![[Pasted image 20250328194832.png]]

Which states you must be able to configure and verify Layer2 and Layer3 EtherChannel using a protocol called **LACP**, Link Aggregation Control Protocol.

#### What is EtherChannel? What problem does it solve ? 
A Layer 2 EtherChannel is a group of switch ports which operate as a single interface,  and a Layer 3 EtherChannel is a group of routed port which operates as a single interface. which you assign an IP to, because it's Layer3.

![[Pasted image 20250328195418.png]]

- We have two switches here, ASW1 is a access switch, a switch that end hosts connect to. DSW1 is a distribution layer switch, a switch that access layer switch connects to.
![[Pasted image 20250328195649.png]]

- The Network administrator notices that the connection to DSW1 is congested, so he decides that he should add another link to increase the bandwidth, so it can support all of the end hosts.
- So, the network admin adds another link and monitors the situation. however, that additional link doesn't seems to help. the connection between ASW1 and DSW1 is still congested and end users are reporting problems.
![[Pasted image 20250328200017.png]]

- So, admin decides to add another link to the connection between ASW1 and DSW1.
![[Pasted image 20250328200103.png]]

- Surely, this will be sufficient.
- The total bandwidth of the connection to the end hosts is still greater than the bandwidth of the connection to DSW1, but that's okay all hosts in the network aren't always in a constant state of sending and receiving internet traffic.
![[Pasted image 20250328200335.png]]

- However, even with three link to DSW1 the congestion doesn't seem any better, so the network admin once again decided to add another link between ASW1 and DSW1
![[Pasted image 20250328200653.png]]

- So now there are four links between ASW1 and DSW1.
- Do you think  things have improved ?
***Well, they haven't, the connection between the two switches is just as congested as before.***

WHY ? 

- Lets say the administrator went to physically look at the port lights of the two switches, what colour do you think they were? 
- Well, the network admin checks DSW1 and all of the port lights are green, so there doesn't seem to be a problem. However, when he checks ASW1  he notices that, of the link connected to DSW1, only one light is green and others are orange.
![[Pasted image 20250328201327.png]]

WHY ??
> Spanning Tree Protocol

- If you connect two switches together with multiple links, all except one will be disabled by spanning tree.
- Other links will be unused unless the active link fails, In that case, one of the inactive links will start forwarding traffic.

WHY ??

>  Well, if all of ASW1's interfaces were forwarding, Layer 2 loops would form between ASW1 and DSW1, leading to broadcast storms.

- So, although having backup link is a good thing, because failure can occur for various reason, it's a waste of bandwidth to have these three interface disabled, not forwarding any traffic.

*However, by forming these four physical interfaces into one logical interface, EtherChannel can solve this problem, giving us both redundancy and increased bandwidth*

- An EtherChannel is represented in a network diagrams by drawing a circle around the interfaces that are grouped together, like this.
![[Pasted image 20250328202731.png]]

- EtherChannel group multiple interfaces together to act as a single interface.
- STP will treat this group as a single interface.
- So, after grouping these interfaces into an EtherChannel the network admin once again checks the link lights. this time they are all green
![[Pasted image 20250328202937.png]]
- Will this cause a Layer 2 loop ? No because this group of four links behaves as if it a single link. 

WHY ?? 

let's say a PC sends a broadcast frame. So it is flooded out all interfaces on ASW1. All of the PCs connected to ASW1 will receive a copy of the frame. Now, Although there are four physical interfaces, they behave as a single interface. so DSW1 will only receive one copy only. This EtherChannel makes these four physical interfaces a single logical interface. ASW1 won't send four copies of the same broadcast frame out of a single interface.

![[Pasted image 20250328203957.png]]

- Same as ASW1, DSW1 will forward the broadcast frame to all of the interfaces, but not to the interfaces in EtherChannel, as all 4 interfaces in EtherChannel all interface behave a single interface. so DSW1 will not forward broadcast frame to the interface it was received on. So, it isn't forwarded back to ASW1, and no Layer2 loop is formed.

![[Pasted image 20250328203613.png]]

- It's kind of like this.
![[Pasted image 20250328204427.png]]

- instead of four separate interfaces, perhaps gigabit ethernet interfaces, connecting ASW1 to DSW1, it's as if there is a single four-gigabit ethernet interface.
- The bandwidth of four separate interfaces in combined to form one faster interface, a virtual four-gigabit interface.

*The difference between the physical characteristics and the logical or virtual characteristics are something you need to know as a network engineer*

For ex. We studied VLANs, Multiple PCs can all connected to the same switch and therefore be in the same LAN, however VLANs virtually divide these PCs into separate virtual LANs, each behaving as a separate LAN. Likewise, these interfaces exit as  four separate physical interfaces, but now they form a single virtual interface. Some other names for EtherChannel are a **PORT Channel** and a **LAG**, which stands for Link Aggregation group.

## How EtherChannel Load balances

- EtherChannel load balances based on flows.
- A flow is a communication between two nodes in the network.
- So for ex. PC1 and SRV1. So let's say PC1 initiates an exchange of data with SRV1, and it sends a few frames to do so.
- The frame is received by ASW1, and assuming it already know MAC address of SRV1, it will forward the  frame out of the port channel to DSW1 however which physical interface it will use ? 
- Well, there is an algorithm it uses to calculate which physical interface the traffic will  actually be sent on, lets say it determines that this  interface should be used.\

![[Pasted image 20250328221419.png]]

- Now, when PC1 sends the next frame in the flow, in the communication between PC1 and SRV1 the same interface will be used to forward the traffic to SRV1.
- So the point is that frame in the same flow will be forwarded using the same physical interface.
- If frame in the same flow were forwarded using different physical interfaces, some frames may arrive at the destination out of order, which can cause problem.
- Some application can deal with frames arriving out of order, but some application can't.
![[Pasted image 20250328221858.png]]

- Now if PC1 want to print something and initiates a separate communication flow with PR1,  ASW1 will once again forward the frame using its virtual port channel interface.
- However, it will make a separate calculation to determine which physical interface will will be used for the flow. for Ex, it might determine that this interface will be used for the flow.
![[Pasted image 20250328222256.png]]

- Just like before, when PC1 sends another frame in the flow, the same member interface of EtherChannel will be used to forward it.
-   How about if PC2 also wants to print something ? it will send the first frame to ASW1 which will then do a calculation to determine which physical interface in the EtherChannel will be used. perhaps this is used.

![[Pasted image 20250328222625.png]]

- So, this is how EtherChannel performs load balancing, by using different physical interfaces in the EtherChannel for different flows.
- The calculation that is done to determine which physical interface to use take into account to few inputs.

*Actually you can change the inputs used in the interface selection calculation*


**Inputs that can be used**

- Source MAC : All frames with the same source MAC address will always use the same interface in the EtherChannel.
- Destination MAC : All frames with the same destination address will always use the same interface in the EtherChannel.
- Source AND Destination MAC :  you can also use both source AND destination MAC addresses.

So for ex :  frames from PC1 to SRV1 will use always a certain interface, frames PC2 to SRV1 will always use a certain interface, which might be same or different than the one used for traffic from PC1 to SRV1, frames from PC1 to RP1 might use another different interface, etc.
The calculation is done based on both the source and destination MAC addresses.

- Source and Destination IP :  You can also configure EtherChannel to select  the interface based on the source IP address, destination IP address or both Source and destination IP address.
- Some Switches also support load-balancing based on the Layer 4 TCP or UDP port numbers, but those are topic for another lesson.
- Also which method the switch can use depends on the Switch model, some may only support using the MAC address, Some may support only IP, some may support both. or some may support all.

![[Pasted image 20250328224030.png]]


## Configuration
*How to check the current load balancing Method*

![[Pasted image 20250330000247.png]]

*How to change the load balancing Method*

![[Pasted image 20250330000442.png]]

*Choices/Method Available on the Switch*

![[Pasted image 20250330000519.png]]

> You use `port-channel` to configure the load-balancing method. however you use `etherchannel` command to view the current load balancing method. Different words are used for the same thing

![[Pasted image 20250330001000.png]]
![[Pasted image 20250330001017.png]]

## Let's start Creating a EtherChannel between two switches.

*There are Three Methods of EtherChannel configuration on Cisco Switches*

![[Pasted image 20250330212122.png]]


## PAgP Configuration 
![[Pasted image 20250330212231.png]]

- First I used the `interface range` command to configure all of the member interface at once.
- This is good idea for EtherChannel, because the configuration on each member interface must match, so if you configure them all at once you can guarantee that.
- To actually configure the EtherChannel use this command, with yet another new keyword `channel-group` followed by a number that identifies the virtual interface, Mode, and than as you can see I used the question mark to see what options there are.
![[Pasted image 20250330212854.png]]

- These two are used for PAgP. 
*DTP used the same modes to form trunks, and the function of each mode is basically the same. 
- Desirable mode actively tries to form an EtherChannel, and auto mode will only form an EtherChannel if the other side is set to desirable, but not if the other side is set to auto. 
![[Pasted image 20250330213307.png]]

- You can see in the figure last line that the virtual port-channel interface was created, with the number we used in the channel group command

- You can see it here in the output of the `show ip interface brif` command.

![[Pasted image 20250330213537.png]]

![[Pasted image 20250330213600.png]]

![[Pasted image 20250330213634.png]]

## LACP Configuration
- it is same configuration like we did for PAgP. just the mode name are different here.
![[Pasted image 20250330213752.png]]

- instead of desirable, LACP uses active mode. and instead of auto, LACP uses passive mode. so 
![[Pasted image 20250330213921.png]]

- In this case I configured this side as active. once again the port-channel interface is created.
- Note that, even if you configure both side as passive, the virtual interface will still be created on each switch. However it won't be functioning as an EtherChannel unless one side is in active mode.

*So, as you can see the commands are basically the same, just the mode names are different*

## Static EtherChannel
- There aren't two separate modes, just one, ON which manually tells these interfaces to form a EtherChannel, this will create a port-channel interface, like before.
![[Pasted image 20250330214545.png]]


____

 - Another command you should know is the Channel-Protocol command. This manually configure the EtherChannel negotiation protocol that the member interfaces should use.
 - This actually isn't very useful command, because you don't need to configure it. IF you configure `Channel-group 1 mode disarable or auto`, the interface will automatically use PAgP, or if you configure the `channel-group 1 mode active or passive` , the interface will automatically use LACP. So, there isn't really a point in using this command.
 - However, I think you should know it for the exam at least, so here's a brief explanation.
![[Pasted image 20250330215203.png]]

*If you try to use mismatched method after configuring the channel-protocol, it won't allow you to do so, you can only apply the support methods by the protocol*

***SO after configuring EtherChannel, you can configure the port-channel interface itself***
> Here the LACP example is used, since all the information is the same regardless of which method you use.

> I also did the same configuration over on DSW1, so the EtherChannel is up and running.

![[Pasted image 20250330215811.png]]

> Below is the output of show running config command.


![[Pasted image 20250330215858.png]]
- There is something interesting to notice here. The trunk configuration that I configured on the port-channel interface were also applied to the  physical interfaces, I didn't manually configure the physical interfaces as trunk.

![[Pasted image 20250330220218.png]]


#### TO verify status of EtherChannel using show command

`show etherchannel summary`

![[Pasted image 20250330220351.png]]

> Flag with Po1 (SU) means
> S -> Layer2 ( switch layer EtherChannel)
> U -> IN use (EtherChannel is Active)

![[Pasted image 20250330220535.png]]

 
>now if we shutdown the port channel, we will se different flag.

![[Pasted image 20250330220629.png]]

> now I changed on of the member interfaces to access mode.

 ![[Pasted image 20250330220951.png]]
> port-channel interface is still up and running and operating but with just 3 interfaces.

#### Another command you can use is
`show etherchannel port-channel`

![[Pasted image 20250330221139.png]]

*One important thing you can check here is EC state which was not in earlier command*

#### Lets see how STP is affected EtherChannel is configured.

![[Pasted image 20250330221312.png]]

As you can see, only the port-channel interface is listed, the physical interfaces do not appear in the output of this command at all. So as I said Earlier, STP is treating these four physical interfaces as a single logical interfaces. instead of blocking 3 of them, they all can forward and receive traffic, with no worries of layer 2 loops.


# Layer - 3 EtherChannel

- Instead of Layer 2 connection between them, let's use a Layer 3 connection.

![[Pasted image 20250330233109.png]]

- Modern network design often leans towards using Layer 3 connections between switches, because that way spanning tree won't be an issue anywhere in the network.
- We could have four switches interconnected in a mesh, and if we're connecting them and if we connecting them with Layer 3 routed ports, all interfaces will be up and forwarding, none will have to be disabled due to spanning tree.
- Now you may be thinking, didn't you just show that EtherChannel means that spanning tree doesn't have to block any ports ? 
- Well, we're just looking at a connection between two switches. Even if we're using EtherChannel, Layer 2 loop can still occur if multiple switches are connected together in a loop.

EX => 
![[Pasted image 20250330233608.png]]

=> All of the connections between switches are using EtherChannel, but if we don't block, any of port-channel interface, broadcast can still loop around the switch like this and cause a broadcast storm.

![[Pasted image 20250330233748.png]]

=> So, spanning tree will block one of the these port-channel interfaces.

![[Pasted image 20250330233834.png]]

=> However, if all of these connections between switches were made using routed ports, not Layer2 switch ports, there is no need to run spanning-tree at all.
=> Routed ports, don't forward Layer 2 broadcasts so no layer 2 loops can be formed.
=> We already know how to configure routed ports with the `no switchport` command

##### Lets see how to configure a Layer3 EtherChannel 
- So starting from a clean configuration, no port-channel have been configured yet on ASW1
- I enter interface range config mode for the member interfaces, and this time before using the `channel-group` command,  I use the `no switchport` command to them Layer3 routed interfaces.
- Then, after using the `channel-group` command,
![[Pasted image 20250330234356.png]]

- I used the show running config to check the config.
![[Pasted image 20250330234425.png]]

- Notice that the port-channel interface that was created has the `no switchport` command to it automatically,
- Now, since we are making a Layer3 EtherChannel, we need an IP address. It should be configured on port-channel interface.
![[Pasted image 20250330234623.png]]

- Now, let's see `show etherchannel summuary` command.

![[Pasted image 20250330234658.png]]

*The Difference is instead of S flag it has the R flag. means it is a Layer3 port channel*

- Now let's check `ip interface brief` 

![[Pasted image 20250330234846.png]]

- In output we can see IP address configured on port-channel 1, so now ASW1 and DSW1 are like two routers connected together.
- They are connected at layer 3 and spanning tree is not running on the connection between them.
- However, just like the Layer2 EtherChannel, traffic will not be load-balanced over the four member interface.


---
![[Pasted image 20250330235230.png]]

---

