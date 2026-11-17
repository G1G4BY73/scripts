 OSI model - PDUs 
 ![[Pasted image 20250215205948.png]]
 Here we have to focus on Layer 3, Network layer  Header.
![[Pasted image 20250215210035.png]]
Learning all fields one by one
##### 1. Version
- Length : 4 bits
- Identifies the version of IP used.
- IPv4 : 4 (0100)
- IPv6 : 6 (0110)

##### 2. IHL
- Length : 4 bits
- The final field of the IPv4 header (options) is variable in length, so this field is necessary to indicate the total length of the header
- Identifies the length of the header in 4-bytes  increments
- Value of 5 = 5 * 4 bytes = 20 bytes
- Minimum value is 5 (=20 bytes) -> with empty options field (option field size = 0 bytes)
*Minimum length of IPv4 header is 20 bytes*
- Maximum value is 15 (15 * 4 bytes = 60 bytes )
![[Pasted image 20250215210922.png]]
*Maximum length of IPv6 header is 60 bytes*
![[Pasted image 20250215211111.png]]

##### 3. DSCP 
- Length : 6 bits
- DSCP : Differentiated Services Code Point
- User for QOS (Quality of Service)
- Used to prioritize delay-sensitive data (streaming voice, video, etc.)

##### 4. ECN
- Length : 2 bits
- ECN : Explicit Congestion Notification
- Provides end-to-end ( between two endpoints) notification of network congestion without dropping packets.
- Optional feature  that requires both endpoint, as well as the underlying network infrastructure, to support it.

##### 5. Total Length
- Length : 16 bits
- Indicates the Total Length of the packet (L3 header + L4 segment)
- Measured in bytes (not 4 byte increment like IHL)
- Minimum value of 20 (=IPv4 header with no encapsulated data)
- Maximum value of 65,535 ( Maximum of 16 bit value)
![[Pasted image 20250215212228.png]]

#####  6. Identification field
- Length : 16 bits
- If a packet is fragmented due to being too large, this field is used to identify which packet the fragment belongs to.
- All fragments of the same packet will have their own IPv4 header with the same value in this field.
- Packet are fragmented if larger the the MTU (Maximum Transmission Unit)
- The MTU is usually 1500 bytes 
- Remember the maximum size of an Ethernet frame ? same 1500 bytes
- Fragments are reassembled by the receiving host.

##### 7. Flag
- Length : 3 bit
- Used to control/identify fragments.
- Bit 0 : Reserved, always set to 0
- Bit 1 : Don't fragment ( DF bit ), used to indicate a packet that should not be fragmented
- Bit 2 : More Fragments (MF bit), set to 1 if there are more fragments in the packet, set to 0 for the last fragment.
- Unfragmented packets will always have their MF bit set to 0.

##### 8. Fragment offset
- Length : 13 bit
- Used to indicate the position of the fragment within the original, unfragmented IP packet.
- Allows fragmented packets to be reassembled even if the fragments arrive out of order.

##### 9. Time to live
- Length : 8 bits
- A router will drop a packet with a TTL of 0
- Used to prevent infinite loops, if a poor routing configuration causes a packet to be continually sent around in a loop, never reaching its intended destination, if enough traffic like that accumulates, it could causes network congestion, and eventually failure.
- originally designed to indicate the packet's maximum lifetime in seconds, 
- In practice, indicates a 'hop count' : each time the packet arrive at a router, the router decreases the TTL by 1
- Current recommended TTL is 0

##### 10. Protocol 
- Length : 8 bits
- Indicates the protocol of the encapsulated Layer 4 PDU.
- Value of 6 : TCP
- Value of 17 : UDP
- Value of 1 : ICMP
- Value of 89 : OSPF (dynamic routing protocol )

##### 11. Header checksum
- Length : 16 bits
- A calculated checksum used to check for errors in the IPv4 header.
- When a router receives packet, it calculates the checksum of the header and compare it to the one in this field of the header.
- if they do not match, router drops the packet.
- Used to check for errors only in the IPv4 header. not in encapsulated data.
- IP relies on the encapsulated protocol to detect errors in the encapsulated data.
- Both TCP and UDP have their own checksum field to detect errors in the encapsulated data.

##### 12. Source IP Address
- IPv4 address of the sender of the packet

##### 13. Destination IP address
- IPv4 address of the intended receiver of the packet

##### 14. Optional
- Length : 0 -320 bits
- Rarely used.
- if the IHL field is greater than 5, it means that options are present.


## Wireshark Packet Capture 
> Ping packet capture with default size 100 bytes

![[Pasted image 20250216185154.png]]

> ping with size 10000 bytes
![[Pasted image 20250216185404.png]]

![[Pasted image 20250216185420.png]]

> ping with df-bit (don't fragment ) # ping 192.168.1.2 df-bit
![[Pasted image 20250216185555.png]]

![[Pasted image 20250216185618.png]]
> here ping fails because the packet were too large to be sent unfragmented, but also not allowed to be fragmented.

