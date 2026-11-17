Virtual Local area network

## What is LAN ? 
LAN is a group of devices ( PCs, Servers, Routers, Switches, etc.) in a single location (Home, office, etc.)
- A more specific definition : A LAN is a single broadcast domain, including all devices in that same domain.
- A broadcast domain is the group of devices which will receive a broadcast frame( destination MAC ffff.ffff.ffff.ffff ) sent by any one of the member.

![[Pasted image 20250323215211.png]]


### Keeping all the system in a single LAN can create some problems
![[Pasted image 20250323215353.png]]

![[Pasted image 20250323215405.png]]

![[Pasted image 20250323215435.png]]

Lets Split up these departments in to separate subnets.

![[Pasted image 20250323215625.png]]

However here there is one problem, The router is going to need an IP address in each subnet, so it will need one interface in each subnet.

So lets replace the single connection between switch and router with three separate connection, one in each subnet. 
- Actually, there is one more efficient way  of doing this by, you don't actually have to use Three separate interfaces, but d
- Don't worry about that for now,

![[Pasted image 20250323220040.png]]

- Still there is one confusion, what if the frame is a broadcast or unknown unicast frame. the switch will flood the frame out of all interfaces.
![[Pasted image 20250323220203.png]]

 - Remember that a switch is only aware up to Layer2. it looks at Layer 2 information like source and destination MAC address only. It doesn't care about Layer 3,4 etc.
 -  So even though there are three separate subnets here the switch doesn't know that. PC1 will send the frame to the switch, it will see the destination MAC address of all  Fs, and then flood the frame.
![[Pasted image 20250323220543.png]]

***This is bad  in terms of both performance and Security***

![[Pasted image 20250323220630.png]]

 Now, one simple solution is to buy a separate switch for each department, however that is not very flexible, and network equipment isn't cheap, so buying one or more switches for every single department could be too expensive, especially for small enterprises. 
 ***however, this is where VLANs  come in.***

Although these PCs are all in same LAN,  we can use VLANs to separate them at Layer 2.
We will assign the Engineering department to VLAN 10, the HR to VLAN20 and sales to VLAN30.
How we configure them on the switch. more specifically on the switch interfaces.
- You configure the switch interfaces to be in a specific VLAN, and the the end host connected  to that interface is part of that VLANs
- The switch will consider each VLAN as a separate LAN and will not forward traffic in that VLANs, including broadcast or unknow unicast traffic. so if we set up these VLANs, if PC1 send this same broadcast frame, after that the frames arrives at the switch it will forward to all the interfaces in same VLAN.
![[Pasted image 20250324174443.png]]

![[Pasted image 20250324174602.png]]

# VLANs
- VLANs are configured on switches on a per interface basis.
- Logically separate end hosts at Layer 2.
- Switches do not directly direct traffic between hosts in different VLANs.

# VLANs Configuration...
![[Pasted image 20250324175019.png]]

> To see the VLANs information in switch.  use 
```
show vlan brief
```
![[Pasted image 20250324175213.png]]
![[Pasted image 20250324175255.png]]

![[Pasted image 20250324175339.png]]

When any VLAN is not already created and you try to add any interface to that VLAN it will automatically create that VLAN.

![[Pasted image 20250324175414.png]]

![[Pasted image 20250324175433.png]]

![[Pasted image 20250324175817.png]]

![[Pasted image 20250324175912.png]]

-----

 ***PART 2***

![[Pasted image 20250324185053.png]]

![[Pasted image 20250324185059.png]]

![[Pasted image 20250324185245.png]]

### Trunk ports
- In a small network with few VLANs, it is possible to use a separate interface for each VLAN when connecting switches to switches, and switches to routers.
- However, when the number of VLANs increases, this is not viable. it will result in wasted interfaces, and often routers won't have enough interfaces for each VLAN. 
- You can use Trunk ports to carry traffic from multiple VLANs  over a single interface.

![[Pasted image 20250324190245.png]]

Lets say this PC in VLAN10 wants to send some data to this other pc in VLAN10. It send the traffic to SW2, which then sends it to SW1. Now here's a question. 
How does SW1 known which VLAN the traffic belongs to ?

**VLAN Tagging**
![[Pasted image 20250324190508.png]]

![[Pasted image 20250324190540.png]]

![[Pasted image 20250324190609.png]]

![[Pasted image 20250324190728.png]]
![[Pasted image 20250324190854.png]]

Note : IEEE 802.3 is Ethernet, another industry standard protocol.

![[Pasted image 20250324190939.png]]

- The dot1Q tag is actually inserted between two fields of the Ethernet header
- dot1q inserts a 4-bytes, or 32-bit field between two fields of this ethernet header.
![[Pasted image 20250324191144.png]]
- The tag consists of two main fields 
	-  Tag Protocol Identifier (TPID)
	- Tag Control Information (TCI)

![[Pasted image 20250324191322.png]]

![[Pasted image 20250324191451.png]]

![[Pasted image 20250324191531.png]]

![[Pasted image 20250324191644.png]]

![[Pasted image 20250324191736.png]]

![[Pasted image 20250324191858.png]]

 ![[Pasted image 20250324191941.png]]
## Native VLAN

- IEEE802.1Q has a feature called the native VLAN
- The native VLAN is VLAN 1 by default on all trunk ports, however this can be manually configured on each trunk port.
- The switch does not add an 802.1Q tag to frame in the native VLAN.
- When a switch receive an untagged frame on a trunk port, it assumes the frame belongs to the native VLAN.
- It is very important that the native VLAN matches in all switches.

![[Pasted image 20250324192449.png]]

### if NATIVE VLAN Mis MATCH between switches
![[Pasted image 20250324192608.png]]


## Lets configure the Switch with trunk configuration
![[Pasted image 20250324192625.png]]

![[Pasted image 20250324192723.png]]

![[Pasted image 20250324192836.png]]

![[Pasted image 20250324192857.png]]

- On switches that only supports dot.1q, you will ONLY need the switch port mode trunk command. but on some switches you will need to set the encapsulation first.

#### TO check the trunk configuration
```
show interface trunk
```
![[Pasted image 20250324193549.png]]


- `Mode on` means that the interface was manually configured as a trunk. 
we will later see how a port can automatically become a trunk without configuration.
- Under that the VLANs allowed are displayed. 
![[Pasted image 20250324193618.png]]

- by default all VLANs, 1 to 4094 are allowed on the trunk. however for security purpose, we might want to limit which VLAN can be forwarded on the trunk, wo we'll look at that configuration next.
-  next is VLAN allowed and active in management domain.
![[Pasted image 20250324193704.png]]

For the last part will learn later as we don't know the spanning tree and all as of now.

here is the command to configure the VLANs allowed on a trunk.
```
int g0/0
switchport trunk allowed vlan 10,30
```

![[Pasted image 20250324194005.png]]

but there are other options to select as well
![[Pasted image 20250324194103.png]]

currently VLAN 10 and 30 are allowed, let's say I also want to add 20, even though no hosts in VLAN 20 are connected to switch1

![[Pasted image 20250324194229.png]]

#### Removing a VLAN from trunk.
![[Pasted image 20250324194346.png]]

- All - option will  do allow all the VLANs in Switch it is same as default.
- except - option will allow all VLANs except the mentioned one]
- none - will not allow any VLAN on trunk
![[Pasted image 20250324194518.png]]

![[Pasted image 20250324194705.png]]

> how to change the native VLAN
> ![[Pasted image 20250324194746.png]]

## Lets see the VLANs information now.
![[Pasted image 20250324194827.png]]

![[Pasted image 20250324194834.png]]

![[Pasted image 20250324194937.png]]

 ______________

Previously we used three separate connections from switch to router  and assigned a separate IP on each one, and each one served as a default gateway address for the PCs in each VLAN.

however now we are using only on physical connection between the two devices. so we must use sub interfaces on router

***Router on a stick - ROAS***

- It is the name used for this method of inter-VLAN routing, as there is only a single physical interface connecting the router and the switch, and looks like a stick on the network topology diagram.
- so in this case that one physical interface begin used on R1 to connect to SW2 is G0/0.
- it is connected to G0/1  on SW2.
- But we can actually divide this one physical interface into three virtual separate sub interfaces. which will allow us to perform inter-VLAN routing with only one single physical interface.

Note : we don't need to do any additional configuration on SW2. We already configured G0/1 as  a trunk, and made sure that VLANs 10,20 and 30 are allowed. that's all you need to do on the switch, configure the interface like a regular trunk.

### Router configuration...
- First make sure that the interface is enabled with no shutdown, as a router interfaces are disabled by default.
![[Pasted image 20250324200342.png]]
- Next up is first sub-interface.
![[Pasted image 20250324200522.png]]![[Pasted image 20250324200533.png]]
- The next command after that is Encapsulation DOT1Q, followed by the VLAN no, which is 10 in this case. this tells the router to treat any arriving frame tagged with the specified VLAN number as if they arrived on this sub interface
- If a frame arrives tagged with the VLAN10, R1 will behave as if it arrived on interface G0/0.10.
- It will also tag all frames leaving this interface with VLAN10 using dot1q

- Finally, after the encapsulation dot1q command,  simply assign the IP address to the sub-interface.
![[Pasted image 20250324201105.png]]

> just for info, here we have assigned the last usable address of the subnet.

That's all for this interface.
Then we have to do the same thing with the other two sub-interfaces.
![[Pasted image 20250324201237.png]]

##### You can confirm the same with `show ip interface brief` command

![[Pasted image 20250324201336.png]]

sub-interfaces appears, as well as the physical interface, although the physical interface it self has no IP address assigned to it.
and below is the routing table.

![[Pasted image 20250324201539.png]]

Notice the connected and local routes are added just like when IP address are added to regular physical interfaces.
when R1 sends frames out of these sub-interfaces, it adds the VLAN tag configured on the sub-interface
For ex. if a packed arrived destined for the 192.168.1.64/26 subnet, it will send packet out of its G0/0 interface tagged with VLAN20.


### Summary 

![[Pasted image 20250324201958.png]]

-----
***PART -3***


![[Pasted image 20250325202524.png]]

> The Native VLAN  feature does have one benefit Because frames in the native VLAN aren't tagged, it's more efficient, each frame  is smaller so it allowed the device to send the more frames per second.

Lets start configuring the native VLAN

#### ON SWITCH
![[Pasted image 20250325201858.png]]

![[Pasted image 20250325201906.png]]

#### ON ROUTER
There are two methods of configuring the native VLAN on a router.
- use the command encapsulation dot1q VLAN-id native on the router sub-interface
- configure the IP address of the native VLAN on the router's physical interface ( the encapsulation dot1q VLAN-id command is not necessary)


**First  lets see one by one**
## 1st

![[Pasted image 20250325203103.png]]

 ![[Pasted image 20250325203230.png]]
#### 2nd

![[Pasted image 20250325210436.png]]

##### Show running config 

![[Pasted image 20250325211907.png]]

##### NOTES
- A multilayer switch or L3 switch is capable of both switching and routing.
- it is Layer 3 aware.
-  You can assign IP addresses to it's interfaces, like a router.
- You can create virtual interfaces for each VLAN, and assign IP address to those interfaces.
- It can be used for inter-VLAN routing

![[Pasted image 20250325212854.png]]
![[Pasted image 20250325212905.png]]
![[Pasted image 20250325212914.png]]

![[Pasted image 20250325213026.png]]

