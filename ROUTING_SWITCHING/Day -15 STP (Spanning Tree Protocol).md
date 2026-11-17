
#### Network Redundancy 
- Redundancy is an essential part of network design.
- Modern network are expected to run 24/7/365. Even a short downtime can be disastrous for a business.
- If one network component fails, you must ensure that other components will take over with little or no downtime.
- As much as possible, you must implement redundancy at every possible point in the network.

**Example or poorly designed Network**
![[Pasted image 20250326170412.png]]

**Example of a better network design**
![[Pasted image 20250326170448.png]]

- However if the Distribution switch / end host connected switch is losses connectivity. unfortunately the all system connected to that switch will lose connectivity due to.
![[Pasted image 20250326170639.png]]

We will cover many protocols that are used to enable network redundancy throughout this course and STP is one of them.

- Spanning tree is  a Layer 2 protocol by the way. 
- It enables redundant layer 2 network.
- So, with in the LAN here, not  routing out to the Internet and between networks at Layer 3

![[Pasted image 20250326171140.png]]


Benefits of redundant LAN, having multiple paths between these switches provides alternate path is one connections fails. However without spanning tree, there is a MAJOR problem here which can destroy you network, So where is the problem. 

> Broadcast Storms

### Broadcast Storms
![[Pasted image 20250326171512.png]]

- Lets say PC1 wants to send some traffic to PC2. to do that it needs to know PC2's MAC address. So let's say PC1 sends an ARP request frame, which is a broadcast frame. it uses the broadcast MAC address of all F's as its Layer2 address.
- When SW1 receives the frame, As it does with broadcast and unknown unicast frames. it will flood it out of all interfaces. except the one it was received on.
- So, SW2 and SW3 both receive a copy of frame. They then do the same thing, they flood it out all interface except the one it was received on.
- So, PC2 receives the APR request and will reply with a unicast ARP reply.
![[Pasted image 20250326172315.png]]

IS EVERY THIS ALL GOOD ? NOOOO....

- Although PC2 receives the ARP request and replies, some broadcast frame will still remain in network and switch will continue flooding them

![[Pasted image 20250326173013.png]]

Let's clean up the others.

![[Pasted image 20250326173054.png]]

- Now, PC2 has received the ARP request and sent reply, but what about these broadcast frames in the network.
- The Switches will continue flooding them. so what will happened next after this.
![[Pasted image 20250326173458.png]]

- SW1 just received the two broadcast frames, on two different interfaces. it will once again flood them.
![[Pasted image 20250326173642.png]]

- Now SW2 and SW3 just received broadcast frames. what they will do? 
![[Pasted image 20250326173816.png]]

- They will flood them.
![[Pasted image 20250326173842.png]]

This will continue forever...….

Do you remember that `ttl` or `time to live` field of IP header ? 

IT is used to prevent infinite loops at layer3, But a Ethernet header doesn't have a TTL filed. These broadcast frames will loop around the network indefinitely. If enough of these looped broadcasts accumulate in the network, the network will be too congested. for legitimate traffic to use the network.  This is called a broadcast storm.
Eventually you network will look like this, so full of looping broadcast frames that no regular traffic can pass through you network.

The red arrows represent the clock-wise loop between the three switches, and purple arrow the counter clockwise loop.

![[Pasted image 20250326174437.png]]

## However...
![[Pasted image 20250326174519.png]]

**So how can we design a network with redundant paths that doesn't result in Layer 2 loops ?**
***Well, Spanning Tree is one answer to this problem***


# Spanning Tree Protocol
![[Pasted image 20250326175015.png]]

 ![[Pasted image 20250326175128.png]]
 So, if we look at this topology again, perhaps these interfaces in green are in forwarding states, while this one on SW3 is in a blocking state, effectively disabling the connection between SW2 and SW3. 
![[Pasted image 20250326181201.png]]

So, effectively its like that link doesn't exist, and this is our topology.
If PC1 send that same ARP request broadcast frame, it will be like this, no more loops.

![[Pasted image 20250326181413.png]]

However, if at some point another interface fails, perhaps this one... the switch will automatically adjust the topology, and the broadcast frame would be flooded like this. again no more loops.

![[Pasted image 20250326181628.png]]


So, that is just a basic outline of the purpose of **Snapping Tree Protocol***.

Now let's go a little deeper into how spanning tree protocol works.

![[Pasted image 20250326181837.png]]

So, back to our topology here, these switches will send BPDUs out of each interfaces.
![[Pasted image 20250326182748.png]]

- They use these BPDUs to advertise themselves to other switches, and to learn about other switches.
- Now, what exactly are these BPDUs used for ? 
![[Pasted image 20250326182902.png]]

Traditionally, the bridge  ID field of the spanning tree BPDU looks like this
![[Pasted image 20250326182948.png]]

As said earlier the switch with the lowest bridge ID become the root bridge, so therefore  by default the switch with the lowest MAC address become the root bridge.

SO here is the topology once again with MAC addresses of each.
![[Pasted image 20250326205840.png]]

- The G0/2 interface on each switch is connected to a PC, so because it isn't receiving any BPDUs, it knows it is safe to go into forwarding mode, there is no risk of creating a Layer 2 loop , so these lights are all green.
- All the three switches have the default priority of 32768, so in order to know which one will be the root bridge we will have to compare the MAC address. Remember the lowest bridge ID wins.
-  here SW1 has the  lowest MAC address. Therefore SW1 will become the root bridge of this network. All ports on the root bridge become designated ports, in a forwarding state.
![[Pasted image 20250326210400.png]]

### Updated Bridge ID 
![[Pasted image 20250326210449.png]]

![[Pasted image 20250326210529.png]]

- By adding the VLAN ID into bridge priority the switch will have a different bridge ID in each VLAN. here is the deeper look at the bridge priority field.
![[Pasted image 20250326210726.png]]

- you may have wondered why 32768 is the default bridge priority. well it's because this total field is 16 bit in length and the most significant bit is set to 1 by default. 
- therefore the default bridge priority was 32768. however with the addition of extended system ID, adding the VLAN ID number to the bridge priority. is changed so the default VLAN ID is 1, therefore the bridge priority in total actually isn't 32768, it is 32769.
![[Pasted image 20250326211313.png]]

## NOW...
![[Pasted image 20250326211340.png]]

![[Pasted image 20250326211508.png]]

![[Pasted image 20250326211526.png]]

- if there are multiple VLAN in the network, the priority would be change.
- we can also change the bridge priority on the switch for a specific VLAN for example. SW1 is the root bridge in VLAN1, SW2 could be the root bridge in VLAN2, and SW3 could be the root bridge In VLAN3
![[Pasted image 20250326211846.png]]
![[Pasted image 20250326212041.png]]

### STEPS STP
![[Pasted image 20250326212324.png]]

![[Pasted image 20250326212332.png]]

 > in our topology all are Gigabit ports so they all have the priority of 4
 
 - The root cost is the total cost of the outgoing interfaces along the path to root bridge.
 - SW1 is the root bridge, so it has a cost of 0 on all interfaces.
 - They are the gigabit ethernet interfaces, but you don't count the cost of receiving interface, just the sending , the outgoing interfaces.
 - So SW1 advertises its root cost of 0 in its BPDUs
 - SW2 will receive the BPDU and add the cost of its outgoing interfaces, G0/1 which is 4, when it floods those BPDUs out of it's interfaces. SW3 will do the same. So, which port do you think SW2 will choose as its root port.
- ![[Pasted image 20250326214145.png]]
![[Pasted image 20250326214607.png]]

![[Pasted image 20250326214624.png]]

![[Pasted image 20250326221354.png]]

What if TWO switches have two connections between them, so both the root cost and the neighbour bridge ID are the same. ? 

- Then we get to final tie-braker, the interface connected to the interface on the neighbour switch with the lowest port ID  will become the root por
![[Pasted image 20250326220922.png]]

![[Pasted image 20250326220932.png]]

![[Pasted image 20250326221424.png]]

So far all our port are in forwarding state both root port and designated ports.

So, to prevent loops do we block both of these ports ?  
Actually no, here's an important rule to remember: every collision domain has a single spanning tree designated port.
Remember, unlike old ethernet hubs, which we don't use anymore, when we use switches, each link is a separate collision domain