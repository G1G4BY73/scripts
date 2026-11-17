
> Things to learn

- What are ACLs ? 
- ACL logics 
- ACL types
- Standard numbered ACLs
- Standard named ACLs

## What are ACLs.
ACLs, Access Control Lists, actually have multiple uses. The name 'access control ' tells us that they control access to network.

> for ex, Host A should be allowed to access Server A, but Host B should not be allowed to access Server A

*Controls which devices have access to which part of network*
> there are some other uses of ACLs, but for now we'll focus on ACLs from a security perspective.

==> ACLs function as a packet filter, instructing the router to permit or discard specific traffic.
 - As we know, if a router receives a packet and it has a route to the packet's destination, it will by default forward the packet according to its routing table. However ACLs let us control that. even if the router has a route to the destination the packet might be discarded if the ACL tells the router to do so.

==> ACLs can filter traffic based on source/destination IP address, source/destination Layer 4 ports, etc.

---
## Configuration 

![[Pasted image 20250415200128.png]]

Let's configure some ACLs without looking at actual configuration.
We shouldn't just configure randomly, we must use them to achieve a certain certain requirement. 

![[Pasted image 20250415200450.png]]

- First up, ACLs are configured globally on the router, in the global config mode.
-  ACLs are made up of an ordered sequence of ACEs, Access Control Entries.

For Ex, to fulfil our requirement we configure ACL1 like this on R1 and R2
![[Pasted image 20250415200816.png]]
> Configuring an ACL in global config mode will not make the ACL take effect.
--> After being created ACL must be applied to an interface.
-->ACLs are applied either inbound or outbound 

If we apply this ACL1 on routers R1's G0/2 interface will it fulfil our requirements.
> NO, Because R1 will only use ACL to filter the traffic exiting G0/2, not entering it.

- If PC3 tires to ping SRV1, when the ping reaches R1 it won't even check ACL, because the traffic is entering G0/2, not exiting it, So R1 will forward the traffic to R2 which will forward it to SRV1.
- Now, when SRV1 sends the reply to PC3, R1 will check the ACL to decide if the traffic should be for forwarded  out of G0/2 or not, IT will check the entries in ACL1 in sequence.

-> If source IP is 192.168.1.0/24. Well, the source is SRV1, so that doesn't apply.
-> if source IP is 192.168.2.0/24. That doesn't apply either, the source is not in that subnet.
-> Permit all other traffic. so it forward the reply to PC3

> PC3 was able to access the SRV1, even though host in 192.168.2.0/24 shouldn't be able to.

![[Pasted image 20250415202406.png]]

Certainly, We didn't apply this ACL correctly.
> What if we applied the ACL inbound to G0/2 ? That means R1 will check all traffic entering G0/2 interface.

Once again it will check the entries in order, from top to bottom.

-> If source IP is 192.168.1.0/24. the source isn't in that subnet.
-> If source IP is 192.168.2.0/24. which is in this subnet 192.168.2.1, so there is a match!

That means R1 will take the specified action, which is deny.
![[Pasted image 20250420232318.png]]

**Once A router finds a match and takes action, it stop checking the other entries in ACL**

*This also does fulfil our requirement but by applying the ACL inbound on G0/2 we are being to restrictive this blocks the host in 192.168.0.0/24 from communicating with all network outside of their local network.*

> PC3 and PC4 can communicate with each other but that's it. so this is not the best way to apply this ACL.

We can apply ACL on R1's G0/0 or R2's G0/0 but lets take a look at best option. The best location to this ACL is outbound on R2's G0/1 interface.
![[Pasted image 20250420233555.png]]

#### Summary 
![[Pasted image 20250420233902.png]]

=> If you apply another ACL it will replace the previous one.

## Implicit deny
![[Pasted image 20250420234108.png]]

# ACL Types.
--> There are two main types of ACLs, and those two types have two subtypes.
![[Pasted image 20250420234232.png]]

![[Pasted image 20250420234312.png]]


## Standard Numbered ACLs
- Standard ACLs match traffic based only on the source IP address of the packet.
- Numbered ACLs are identified with a number( i.e. ACL1, ACL2, etc)
- Different types of ACLs have a different range of numbers that can be used.
---> Standard ACLs can use 1-99 and 1300-1999.

![[Pasted image 20250420235513.png]]

#### Configuration
![[Pasted image 20250420235746.png]]

![[Pasted image 20250420235758.png]]

Example of these configurations
![[Pasted image 20250423140455.png]]

Requirements
![[Pasted image 20250423140531.png]]

Configuration required.
![[Pasted image 20250423140814.png]]
![[Pasted image 20250423140936.png]]
![[Pasted image 20250423142020.png]]

> this is the example of numbered ACLs

### Standard Named ACLs
- Standard Named ACLs also match traffic based only on the source IP address of the packet.
- However instead of a number they are identified with a name.
![[Pasted image 20250423142318.png]]

- With entry number function you can manually specify the entry number to control the order of entries.
![[Pasted image 20250423142607.png]]

![[Pasted image 20250423142637.png]]

Example for named ACLs, but this time of R2 with same topology.
![[Pasted image 20250423142753.png]]

Requirements
![[Pasted image 20250423142805.png]]

Configurations
![[Pasted image 20250423143225.png]]

![[Pasted image 20250423143313.png]]

![[Pasted image 20250423143410.png]]

Note: If you apply multiple ACLs on a interface in a direction it will replace the previous one. each interface can have one ACL applied on each direction.
ACLs entries are processes in order not by priority. 