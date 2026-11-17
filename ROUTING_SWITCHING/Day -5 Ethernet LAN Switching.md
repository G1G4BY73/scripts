#### What is LAN ? 
There are different ways of defining a LAN, but basically it's a network contained within a relatively small area, like an office floor, or your home network.
>Routers are used to connect separate LANs.

![[Pasted image 20250114223829.png]]

Switch operates at Layer 2 and, Layer 2 PDU is a frame. Today we are going to focus on how switches receive and forward frames, specifically Ethernet frames, since it's the layer 2 protocol used in virtually every LAN in existence today.

Let's take a look at the actual content of this Ethernet header and trailer.
![[Pasted image 20250114224317.png]]
here's an Ethernet frame, encapsulating the packet with a header and trailer.

#### what's are there in Ethernet Header and Ethernet Trailer.
![[Pasted image 20250115195610.png]]

##### Preamble : 
- Length : 7 bytes (56 bits)
- Alternating 1's and 0's
- 10101010 * 7 
- Allows devices to synchronize their receiver clocks to make sure that they are ready to receive the rest of the frame and the data inside.

##### SFD : 
- 'Start Frame Delimiter'
- Length : 1 byte (8 bits)
- 10101011
- Marks the end of the preamble, and the beginning of the rest of the frame

##### Destination & Source
- Indicate the device sending  and receiving the frame
- Consist of the destination and source 'MAC addresses'
- MAC = Media Access Control
- 6 bytes (48 bits ) address of the physical device
>This is separate from a logical address like a IP address, the MAC address is actually assigned to the device when it is made.

##### Type / Length
- 2 bytes (16 bits ) field.
- It can be used to represent either the type of the encapsulated packet, or the length of the encapsulated packet.
- If A value of 1500 or less in this field indicates the length of the encapsulated packet (in bytes)
- If A Value of 1536 or greater in this field indicate the Type of the encapsulated packet (usually IPv4 or IPv6), and the length is determined via other methods

IPv4 = 0x0800 (hexadecimal) = 2048 in decimal
IPv6 = 0x86DD (hexadecimal) = 34525 in decimal

### Try to remember the length of each field of Header and trailer
![[Pasted image 20250115202054.png]]

## Ethernet Trailer
##### FCS
- 'Frame check sequence'
- 4 bytes (32 bits) in length
- Detect corrupted data by running a CRC algorithm over the received data
- Ethernet frame's Frame check sequence is a cyclic Redundancy check.


## MAC Address properties
- 6-byte (48-bit) physical address assigned to the device when it is made.
- A.K.A. 'Burned-In Address' (BIA)
- Is globally unique
- The first 3 bytes are the OUI (Organizationally Unique Identifier), which is assigned to the company making the device.
- The last 3 bytes are unique to the device itself
- Written as 12 hexadecimal characters.


## MAC Address Understanding and working

![[Pasted image 20250123214632.png]]

- Here the first half (OUI), organizational unique identifier, which is the first half of the mac address is AAAAAA for each device, so we know that these PCs are from the same Maker.
- The Second half of the MAC address of each device, however is, different for each PC.as the second half identifies the device itself.
- Now lets say PC1 want to send some data to PC2.
![[Pasted image 20250123215109.png]]
>*Unicast Frame*  : a frame destinated for a single target (PC2 in this case)

- PC1 sends the frame through it's network interface card, which is connect to SW1, and SW1 receives the frame.
- After SW1 receives the frame, it looks  at the source MAC address field of the frame and then uses that information to LEARN where PC1 is.
- As you can see below, it adds the MAC address AAAA.AA00.0001 to it's MAC address table. and it associates that MAC address with it's F0/1 interface.
![[Pasted image 20250123215557.png]]
> This is known as *Dynamically learned MAC address* / Dynamic MAC address. because it wasn't manually configured on the switch, the switch learned it itself.

- Every switch will keep a MAC address table like this, and they fill the MAC address table dynamically by looking at the source MAC address of frame it receives. 
- Since SW1 received a frame from source MAC address 0001 on it's F0/1 interface, it knows that it can reach that MAC address on the interface. and adds  it to MAC address table. 
![[Pasted image 20250123220211.png]]

- *Now there is one problem. ?*

- The destination of the frame is AAAA.AA00.0002 but SW1 doesn't know where that is. This, by the way is called an *unknown unicast* frame,  a frame for which the switch doesn't have an entry in it's MAC address table.
- Because the switch doesn't know how to reach the destination, it has only one option. This it to FLOOD the frame.
- Flood means to forward the frame out of All of its interfaces, except the one it received the packet on.  
![[Pasted image 20250123220918.png]]
- SO, that would look like this. SW1 copies the frame and sends it to its F0/2 and F0/3 interfaces. It doesn't send it out of its F0/1 interface, because that's the interface it received the frame on. So what happens next ? 
- Well, PC3 ignores the packet, because the destination MAC address doesn't match its own MAC address it simply drops the packet.
![[Pasted image 20250123221205.png]]

- PC2, However receives the packet, and then processes it normally, up the OSI stack. However Unless PC2 sends a reply of same sort, it stops there.
- SW1 never receives a packet reply from PC2, so can't learn PC2's MAC address and use it to populate the MAC address table. So let's say PC1 sends another frame to PC2, once again, it will received by SW1, and it already knows the PC1's MAC address, so it doesn't have to add it to the MAC address table again. However still it doesn't know where PC2 is, so it once again floods the frame. PC3 drops the frame and PC2 will process it. 
- Now suppose this time PC2 want to send a reply to PC1, so PC2 sends the frame out of network interface, and SW1 receives it. SW1 looks at the source MAC address of the frame, and then adds it to its MAC address table, associating it with it's F0/2 interface. This time, however, it doesn't flood the frame. This is know as a *known unicast frame*, because the destination is already in its MAC address table.
![[Pasted image 20250123222702.png]]
- Whereas UNKNOWN unicast frames are flooded, known unicast frames are simply forwarded to the destination and PC1 then process the frame up the OSI stack, through the de-encapsulation process.
![[Pasted image 20250123222733.png]]
> On Cisco switches, these MAC addresses are removed from the MAC address table after 5 minutes of inactivity. 

- SO if PC1 didn't sends any traffic for over 5 minutes, SW1 would remove the MAC address to clean out the MAC address table, Of course, if PC1 sent traffic again, SW1 would dynamically lean its MAC address again.

## Another Example.
![[Pasted image 20250123223242.png]]

- Here PC1 want to send some information to PC3. The source MAC address of packet is 0001 and destination is 0003.
- So, PC1 sends the frame out of its network interface and it arrives at SW1, SW1 learns PC1's MAC address from the source address field of the frame. and associates it with the interface on which it was received, F0/1.
![[Pasted image 20250123223514.png]]
- Now, SW1 has learned the PC1 can be reached via F0/1 interface, but it still doesn't know where PC3 is, unknow the type of this frame is called *unknown unicast frame* and what does a switch do with an unknown unicast frame ? = FLOOD. it floods it out of its all ports, except the one it was received on. In this case it will flood the frame out of F0/2 and F0/3 but not F0/1. because it was received on F0/1.
- PC2 drops the frame because the destination MAC address does not match its own MAC address.
![[Pasted image 20250123224614.png]]
-  Now what will SW2 do? Well, the exact same rules apply, just like SW1 did, it uses the source MAC address field of the frame to dynamically learn PC1's MAC address and the interface it use to reach PC1.
- Note that, unlike on SW1, PC1 isn't  actually directly connected to the interface. SW2 enters in its own MAC address table. However,  this is the interface which SW2 will use to reach PC1. That's the meaning of the interface in the MAC address table, it doesn't mean the device is directly connected to this interface. 
- Now SW2 received a unicast frame, that is a frame destined for a single device, but it doesn't know how to reach that device, because its not in its MAC address table.
- One last time this kind of frame know as unknown unicast frame.  and switch floods it out all interfaces, except the one it was received on. so which interface will it send the frame out of ?   Well it received the frame the frame on F0/3, so it won't be flooded out of that interface, but it  will be sent out of all other interfaces, F0/1 and F0/2 in this case.
![[Pasted image 20250123225753.png]]
- PC4 drops the frame because the destination MAC address doesn't match its own. PC3 however, receives the frame, as the destination MAC address matches its own MAC address.
- Lets say PC3 wants to send  a reply back to PC1. Notice the source and destination MAC addresses are reversed. PC3 sends the frame out of its network interface, and it is received by SW2. 
- SW2 learns the PC3 MAC address, and enters the MAC address and corresponding interface in its MAC address table.
- Just so we're clear, the switch uses the SOURCE MAC address to fill its mac address table because, if it receives a frame from that source on the interface, it knows that, it can reach that MAC address via that interface.
![[Pasted image 20250123230455.png]]
- SW2 already has an entry for the destination MAC address, 0001 in its MAC address. so there is no need to flood the frame. instead, it is forwarded normally out of the corresponding interface in the MAC address table, which is F0/3, the frame is received by SW1, which adds an entry for PC3's MAC address in its MAC address table. with the interface F0/3, since that's where it received the frame.
- Finally since SW1 already has an entry for the destination MAC address in its own MAC address table, SW1 forwards the frame out of the corresponding interface, and it reaches its destination PC1.

### Day -6 Continue.. Ethernet LAN Switching

![[Pasted image 20250124102838.png]]
![[Pasted image 20250124102904.png]]


## Example -3 
![[Pasted image 20250124103501.png]]

- Here the interfaces are not fast Ethernet this time they are gigabit ethernet, hence the G0/0, G0/1, and G0/2 for each switch MAC addresses are more realistic.
- Notice once again OUI, the first half of MAC address that identifies the make of device is same for each PC. Of course, the second half of each device's MAC address is unique, since that represents the device itself. but we will refer we will use last 4 digits to keep thigs simple.
-  Let's add some IP addresses in this example. We won't actually talk about the details of IP addresses in this example, but we'll need IP address to explain something else.
- So, Lets say 192.168.1.0./24, represents this whole network here. .1 represent PC1, PC2 =.2, PC3=.3 and PC4=.4
![[Pasted image 20250124143422.png]]
- So when a device sends some data to another device, it doesn't just include a source and destination MAC address. Encapsulated within that frame is an Internet protocol, know as IP, Packet, and that IP packet Includes a source and destination IP address.

>when you send data to another computer, you enter the IP address, not the MAC address.

- So, the user entered the IP address 192.168.1.3 as the destination, but PC1 has to discover PC3's MAC address by itself.
- Remember, these switches are Layer2 devices, they don't operate at Layer3, so they need to use MAC addresses not IP addresses. So, PC1 wants to send this ethernet frame to PC3, but first it has to learn PC3's to do so, it uses something called ARP, the **Address Resolution Protocol**

## Address Resolution Protocol (ARP)
- ARP stands for **Address Resolution protocol**.
- ARP is used to discover the Layer2 address (MAC address) of  a known Layer3 address(IP address)
- The ARP process consist of two messages. The **ARP request** sent by the device that wants  to know the MAC address of the other device. And the **ARP reply**, which is sent to inform the requesting device of the MAC address.
- The ARP request is sent as a broadcast ethernet frame. *Broadcast means it is sent to all hosts on the network*, Because the destination host is unknown, it is broadcast the request and wait for the reply from the correct device. 
- The APR reply is unicast. A unicast frame is sent to only one host, in this case its the host that sent the ARP request.
![[Pasted image 20250124210840.png]]

Let's see now how this works in our network.
- Here at the bottom is the original frame that PC1 wants to send to PC3, but first, It has to send this frame.
![[Pasted image 20250124211128.png]]

This is an ARP request frame, The source and destination IP address, as well as the source MAC address, are the same however, look at the destination MAC address **FFFF.FFFF.FFFF**
 is the broadcast MAC address. This is the destination MAC address used when a device want to send ethernet frames to  all other devices on the local network. 
 - So, PC1 has prepared the ARP request to be sent. it sends the frame out of its network interface, and it is received by SW1.
![[Pasted image 20250124211515.png]]
- SW1 then adds PC1's MAC address to its MAC address table. So, since the destination MAC address is all Fs, SW1 broadcasts the frame out of its all interfaces , except the one it was received on. This is very much like a switch does with an unknown unicast frame.
![[Pasted image 20250124212000.png]]
- PC2 receives it, but it ignores the frame, That's because the destination IP address doesn't match PC2's IP address, so it  know that the ARP request is meant for a different PC, so it just ignores the request.
- Of course, then SW2 learns PC1's MAC address and adds it to the MAC address table. associating it with G0/2 interface since that's the interface the frame was received on.
- Since the destination MAC address is the broadcast MAC address, SW2 also sends the frame out to all interfaces, except the one the frame was received on. that means it sends the frame out of G0/0 and G0/1. 
- PC4 ignores the frame, because the destination IP address doesn't match its own. However, PC3 recognize that the destination IP address does match its own IP address. so it doesn't ignore the APR request. What is does it send the other APR message  I mentioned briefly, the APR reply. below you can see the ARP reply packet.
![[Pasted image 20250124212802.png]]
- The source IP is PC3's IP, and the destination is PC1's IP. The source MAC is PC3's MAC address, and the destination is PC1's. Although the ARP request message was broadcast message, because PC1's MAC address was used as the source MAC address of the ARP request message, PC3 knows PC1's MAC address. So it can send the ARP reply directly to PC1, without having to broadcast the frame.
- PC3 sends the frame out of its network interface, and it is received by SW2, SW2 learns PC3's MAC address and enters it into the MAC address table, associating it with the G0/0 interface. Since this a unicast frame and SW2 already has entry for the destination MAC address. in its MAC address table, what kind of frame is this, and what does SW2 do with it? 
- Its a known unicast frame and SW2 will simply forward it out of the interface in MAC address table, it will not flood it like an unknown unicast frame or a broadcast frame.
- Because SW2 learned PC1's MAC address on the G0/2 interface, it will send the frame out of that interface toward SW1.
- SW1 receives the frame, and since it already learned the PC1's MAC address on the G0/0 interface, it simply forward the frame out of the interface and PC1 finally receive the APR reply. PC1 will then use that information to add an entry for PC3 to its APR table, which is used to store the IP address to MAC address associations.
- Lets take a look at an ARP table.
![[Pasted image 20250124214314.png]]
