
## What is a Network Model ? 
Networking models categorize and provide a structure for networking protocols and standards.
*protocol is a set of rules defining how network devices and software should work*

> protocol is a set of logical rule not physical standards.

### OSI Model
![[Pasted image 20250113190401.png]]
## OSI Model Layers

## Layer -7

![[Pasted image 20250113190539.png]]

## Layer -6
![[Pasted image 20250113190821.png]]

## Layer -5
![[Pasted image 20250113191956.png]]

**NOTE**
![[Pasted image 20250113192117.png]]

## Layer -4
![[Pasted image 20250113192323.png]]

![[Pasted image 20250113192703.png]]

## Layer -3
![[Pasted image 20250113192428.png]]

![[Pasted image 20250113192515.png]]

## Layer -2
 ![[Pasted image 20250113192637.png]]
 ![[Pasted image 20250113192740.png]]
## Layer -1
![[Pasted image 20250113192921.png]]

 Now  the data is not further encapsulated at layer 1 this is then sent over the connection whether it's a electrical signal over a wire or a radio/ wireless signal in case of wi-fi, to the neighbouring system. once it is reached the remote device  the reverse process of encapsulation, de-encapsulation, takes place,
 - the data link layer translate the raw physical data into a complete frame once again.
 - Then the layer 2 header and trailer are removed leaving the layer 3 packet. then the layer 3 header is removed  leaving the layer 4 segment. finally, the layer 4 header is removed and we left with the original data prepared by the upper layers of the original device. that is the process of de-encapsulation.
## PDU 
![[Pasted image 20250113193813.png]]

*Example for referring. packet is term for packet 3 PDU.*

# TPC/IP Suite
![[Pasted image 20250113194135.png]]

![[Pasted image 20250113194218.png]]

![[Pasted image 20250113194415.png]]

### Notes -
- Adjacent-layer interaction refers to interaction between different layers of OSI Model.
- Same-layer interaction refers to interaction between the same layer on different hosts, in this case layers of youtube's web server and the browser on your PC.
- The concept of Same-Layer Interaction allows you to 'ignore' the other layers involved and focus on interaction between a single layer on the different devices.
- Packet refers to he OSI Layer 3 PDU. It would have two headers(Layer 4 header, Layer 3 Header).
- Segment refers to the OSI  layer 4 PDU. it would have one header (layer 4 header), and no trailer.
- Frame refers to the OSI Layer 2 PDU. It has three headers (Layer 4, Layer 3 and Layer 2 headers) and one trailer (Layer 2 Trailer)
- Although the network layer is very relevant to network engineer, it is not the only one.
-  Application-Presentation-Session, these three layers of the OSI model are not generally relevant to the role of a network engineer. They are relevant to application developers.
- The Lower four layers. Transport-Network-Datalink-Physical are all very relevant to the role of a network engineer.
- The link layer of TCP/IP model is equivalent to Data Link and physical layer of OSI model.
- The Transport Layer Provides the Host-to-Host Communication.