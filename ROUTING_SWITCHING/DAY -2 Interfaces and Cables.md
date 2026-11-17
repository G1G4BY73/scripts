

![[{246C5D2B-EE48-4F37-875F-F1E451C802D7}.png]]

> RJ45 ports -- Registered Jack 45

![[{940810ED-43B7-410F-9763-AAA52381C06C}.png]]

> RJ45 Connectors

The RJ-45 Connector is used on the end of a copper Ethernet cable. 

### What is Ethernet ?  
- Ethernet is a collection of network protocols/standards.
- For the purpose of this lesson, we will focus of types of cabling as defined by Ethernet standards.
-  As said there are many standards included in Ethernet, but the focus of this lesson is interfaces and cables.

##### Why do we need network protocols/standards ? 
![[{E1BA4CF4-F61F-4BDE-B2FF-2A5D4E47783C}.png]]

![[{966A8145-CF08-4D82-8F88-8B5FB71046C0}.png]]

>If you are trying to connect to, for example a network switch, but the maker of the cable and the maker of the switch haven't agreed upon the size and shape of the connector and port you won't be able to connect them.

 That's why there are industry standards that all vendors follow, both in terms of  physical standards like connectors and cables like these, as well as logical standards like IP, the internet protocol.


#### Bits, Bytes & Speed 

Now, Connections between devices in a network operate at a set speed. These speeds are measured in bits per seconds. A bit is a value represented by either a 0 or a 1. a series of 8 bits is a byte.

- Speed is measured in bits per seconds (kbps, Mbps, Gbps, etc), not bytes per seconds because imagine if a byte , 8 bits of data being sent along a wire. It reaches the neighbouring device one bit at a time. not one byte at a time.  
- Data on a hard drive for example is measured in bytes, so remember a Gigabyte is actually 8 times larger than a gigabit.

![[{C8314AE2-3DE3-49DA-856D-C776DEFA7FC0}.png]]

## Ethernet Standards
- Defined in the IEEE 802.3 standard in 1983
- IEEE = Institute of Electrical and Electronics Engineers

![[{E3943CA5-DA64-4098-9D54-3505F6A48127}.png]]


### UTP Cables
- The copper cables used in Ethernet standards are UTP cables. UTP stands for Unshielded Twisted Pair.
- Unshielded means that the wires have no metallic shield, which can make them vulnerable to electrical interference.
- The twist actually helps against electromagnetic interference, or EMI.
- There are four pairs of wires twisted together, that makes eight wires in total. 

![[{4FB68807-1BDE-4251-A5B5-D38A556F51C9}.png]]

![[{CB776B21-2858-4D8D-9EB8-41324C8265E4}.png]]

#### PC to Switch 2 pair (10/100 Mbps)
![[{EB991F53-BC1C-4393-8DA3-357A2EF80C72}.png]]
### What is Full-Duplex ?
Full-Duplex transmission means that both devices and send data at the same time, and problems like collision will occur because they use separate to transmit and receive data.

#### Router to Switch 2 Pair (10/100 Mbps)
![[{90024C06-764B-4317-B969-1D7715C2FF65}.png]]

When connecting a PC to a Switch, or Router to a switch, this works fine. Because they transmit and receive on opposite pin pairs, a regular cable like this works well.

> This kind of cable, by the way, is called a straight-through cable. because pin 1 on one end connects straight through to pin 1 on other end. Pin 2 connects to Pin 2, Pin 3 connects to Pin3, etc.

Now, in Networks we don't always connect PC to switch, switch to router. what if we want to connect a router to another router or may be a switch to another switch ? or may be connect two PCs together  ? 

> Now if we replace the switch on the right with another router. what will happen what if router on the left send data to the router on the right ? 

![[{458835B7-0934-49BB-80A9-417394BF69A7}.png]]

>  Well, it's simply not going to work.

The right router isn't prepared to receive data on Pins 1 and 2 of its interface. So communication between  the two routers just doesn't happen.

###### So, How we can successfully connects two routers together ? or perhaps two Switches or two PCs  ?
> Same thing applies to connecting a PC directly to a router, also, because they both transmit data on Pin 1 and 2, and receive data on Pin 3 and 6.

The Answer to this problem is a different type of cable . A CROSSOVER cable.

- In a crossover cable, a pin on one end of the cable doesn't connect straight to the same Pin on the other end.
- Essentially, the pairs are reversed on each end.
- So the Pin 1 on one side connects to the Pin 3 on other side and Pin 2 on one side connects to Pin 6 on other side. As you can see the two pairs, 1 and 2, and 3 and 6, are reversed. like wise Pin 3 on one side connects to Pin 1 on other side and Pin 6 on one side connects to Pin 2 on other side.

> The wires are crossed over each other, hence the name 'crossover cable'. in summary The transmit pins on one side connected to receive pins on other side, so the two device can send data to each other with no problem.

##### Switch to Switch
![[{81CE1BF0-C247-4A36-A4A7-CC7520FB80CB}.png]]

##### Router to PC
![[Pasted image 20250107212835.png]]

![[{9F388127-1835-4ADD-AC8F-A313968600AB}.png]]
> Hence we also can't connect a router or a PCs directly to Firewall.


# Auto MDI-X 

 Well of that is Import Information to know, and cause issues in network even in modern days, the truth is that most modern Networking devices have evolved beyond having to worry straight-through or crossover cables. That's  because newer networking devices include a feature called Auto MDI-X.

- Auto MDI-X allows devices to automatically detect which pins their neighbour is transmitting data on, and then adjust which pins they use to transmit and receive data. 
- Then they can exchange data normally.
- So, Unless you're working with network equipment that is quite old, you don't really have to worry about straight-through and crossover cables.
- But this is good information to be aware of, and it's good to learn about the concept of Auto MDI-x, both for the exam and real world networking.

# UTP Cables 4 pair (1Gbps/10Gbps)
## High Speed Copper Ethernet cables

![[{D8284A82-2980-49F7-9305-464ACD3B36B3}.png]]

- For Gigabit Ethernet and 10 Gigabit Ethernet, all 8 wires are used.
- The last 2 pair are 4 and 5, and 7 and 8.

> Another big difference between 10 Mbps & 100Mbps and  1 Gbps & 10 Gbps. In addition to using all four pairs of wires, in 1Gbps & 10Gbps, Each pairs of wire are bidirectional, meaning each pair isn't dedicated specifically to transmitting data or receiving data
> This is part of the reason that they can operate at much faster speeds.

##### We have covered a lot about connections using copper UTP cables. but there is a newer technology that is superior in many ways for example. 
- Copper UTP wiring can be used for up to 100 meters. that's usually plenty within a LAN, but how about for larger networks ? 

# Fiber-Optic Connections
![[{07D6E59B-982D-4930-B9BA-5B6B46666FFD}.png]]

>These are SFP interfaces, In these interfaces you insert something called SFP transceiver
![[{D686FA89-DE36-4219-B107-0465731BABB7}.png]]

Still this connection is Incomplete. What kind of cable connects to one of these ? 
![[{2A3F2B56-518F-4FDD-B81A-A584B2F125CC}.png]]

## Fiber Optic Cable
- Rather than an electrical signal over copper wiring, these cables send light over glass fibers.
-  There are two connectors on each end. That is because you need one connector to transmit data, and one to receive data on each end.
- Copper UTP cables uses separate wire pairs within the cable to transmit and receive data. The fiber optic cables instead use separate cables to transmit and receive. 
- Transmit on the left connects to receive on the right and transmit on the right connects to receive of left.
![[{3EEC7EAD-CE52-425D-9EF9-1AD00C0DC0F7}.png]]

## Structure of the Fiber-Optic Cable
Cable is divided in four number of parts from the centre to the outer layer
- 1 : the fiberglass core : Light is transmitted down this core to transmit data from one device to another. no 2 : is  Cladding that reflects light, no 3 : is a protecting buffer which protects the fiber glass from breaking and no 4 : is the outer Jack of the cable.
![[{F1D47E11-0F46-4F01-B08F-F7F8FA3ADAF2}.png]]

#### There are couple main types of fiber cable.
1. Single-mode fiber 
2. Multimode fiber

### Multimode Fiber Cable
![[{26DB318C-743C-4669-853F-B139C22538BC}.png]]
> Centre represent the fiberglass core and the blue represents the reflective cladding.


- In Multimode fiber, the core the actual glass fiber is wider then the single mode fiber.
- this wider core allows multiple angles(modes) of lights waves to enter the fiberglass core.
- Multimode fiber cables allow longer cable than UTP, but shorter than single-mode fiber.
- These cables are cheaper than single-mode fiber ( due to cheaper LED-based SFP transmitters)

### Single-Mode Fiber Cable
![[{F81C46AF-9BED-42E2-94DE-52BBE8D86365}.png]]

> Centre represent the fiberglass core and the blue represents the reflective cladding.

- Core diameter is narrower than a multimode fiber.
- Light enters at a single angle(mode) from a laser-based transmitter.
- Allows longer cables than both UTP and multimode fiber
- Single-mode fiber cables are more expensive than multimode fiber cables due to the more expensive laser based transmitter used.

*Now let's look at some standards for fiber-optic cables like we did with UTP cables.*

![[{860CF15D-D0E7-43DF-9DE1-1FCE693302AC}.png]]

> UTP vs Fiber-Optic cabling.
![[{70732A08-C2BA-4350-8DD4-6D7527EC3B16}.png]]


