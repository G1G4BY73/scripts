![[Pasted image 20250215164208.png]]

![[Pasted image 20250215164227.png]]

> this is default status, switch is with default configuration, just hostname SW1 is given.

> Here unlike routers, switch don't have `shutdown` command applied by default.

> see the connected device/ interface are in up up state. with no configuration required.

 > Switch interfaces are Layer 2 device hence doesn't require any IP address.
 
 ![[Pasted image 20250215164924.png]]
Import command to check switch interface status
```
show interface status
```
![[Pasted image 20250215165057.png]]
> name is same as description in router 

**A full duplex is the device is capable of send and receive data at same time**
> a denotes auto negotiated 

*Speed is also auto negotiated with a speed of 100mbps* 
> auto means switch a auto select speed up to 10/100mbps but other device support 100mbps so it auto select the speed to 100 mbps.(fastest)

> interface type is RJ46 ethernet ports Ethernet UTP 10/100  is the speed on which these interface can operate.

- Auto negotiation works well so usually you'll leave it be, lets go and manually configure the speed and duplex of an interface, F0/1 which is connected to R1

Now 

Enter into config mode and select interface
```
conf t
int f0/1
```
![[Pasted image 20250215170403.png]]

Now if you check the interface status again you will see diff.
![[Pasted image 20250215170446.png]]

> lets configure some description to other interfaces of switch
![[Pasted image 20250215170555.png]]

Now, How about the unused interfaces ?
> Although the fact that switch interfaces are enabled by default is convenient, as you can just plug a device in and straight away, it can be a security concern. Really you should disable the interfaces.

>Fortunately, instead of having to configure each of 8 of the interfaces only by one, there is a way to configure all 8 interfaces at once.

From global config mode type to enter interface range config mode.
```
interface range f0/5 - 12
```
```
description ## Not in use ##
```
```
shut down 
```
![[Pasted image 20250215171117.png]]

*The interfaces in the range don't all have to be consecutive*
For ex, if  I later want to enter fastethernet 0/5,6,9,10,11 and 12 but leave 7 and 8 shut down I can do.
```
int range f0/5 - 6, f0/9 - 12
```
![[Pasted image 20250215171422.png]]

Now, lets check the show interface status again.
![[Pasted image 20250215171508.png]]

> disabled and administratively shutdown means the same thing.

## Half/ Full duplex
![[Pasted image 20250215171711.png]]

*In modern network device. all can use full duplex on their interfaces*
So, you may wondering where is half-duplex used ? 
> in modern day network all most no where but just for learning. see the below example.

Let take a network device which was used before switch. A Hub. The hub is much simpler than a switch, in fact it is simply a repeater.
![[Pasted image 20250215172216.png]]
- Any frame it receive, it floods like a switch with a broadcast or unknown unicast frame.
-  For example if PC1 wants to send a frame to PC2, it will send the frame out of it's all interfaces to PC2 and PC3. 
- PC3 will recognize that the destination MAC address is not its won and ignore the frame, and PC2 will receive it normally.
- What if 2 PC wants to send a frame at same time. In this cast PC1 is trying to send a frame to PC2, and PC3 is trying to send a frame to  PC1, at this is where problem occurs.
- The hub won't send the one first and then send the other after, it simply tries to flood both at same time. and this will result in a collision on the interface, and PC2 won't receive either frame intact.
![[Pasted image 20250215173014.png]]

- To deal with the collision in a half duplex situation like this, Ethernet devices use a mechanism  called 'CSMA/CD'.
- CSMA/CD stands for `carrier sense multiple access with collision detection`
- It describes how devices avoid collision in a half duplex situation, and how they react if  collision do occur. it work like this.
- Before sending frames, devices listen to the collision domain until they detect that other devices are not sending frames. 
- If a collision does occur, which can still happen because of bad timing and such, the device sends a jamming signal to inform the other device that a collision happened.
- Each device then waits a random period of time before sending frames again.
-  Now, that process works, and it was how network operated for a long time.
![[Pasted image 20250215174617.png]]

- But switches are more sophisticated then hub.
- Hubs are simple repeaters which operates at layer 1, repeating whatever signals they receive.
- Switches operates at layer 2, using layer 2 addressing, MAC addresses, to send frames to specific hosts. They also won't try to send two frames to the same host at once.
- So, This network which was one collision domain when connected to a hub, is now 1,2,3 collision domains. 
![[Pasted image 20250215175130.png]]
 - Because of the improve functionality of switches over hubs, these devices can operate in full duplex, meaning they don't have to worry about wheatear or not other device are sending data at the same time.
 - Although problem like collision still do occur, they are rare and usually are a sign of a problem, like a misconfiguration, rather than a regular occurrence like in a half duplex.

#### Speed/Duplex Autonegotiation on interfaces.
![[Pasted image 20250215203848.png]]

### *What if autonegotiation is disabled on the other device connected to switch*
![[Pasted image 20250215204129.png]]

![[Pasted image 20250215204434.png]]

##### Checking Counters & Error/stats in interfaces 
![[Pasted image 20250215204728.png]]

![[Pasted image 20250215204700.png]]
