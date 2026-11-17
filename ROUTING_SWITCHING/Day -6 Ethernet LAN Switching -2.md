
### Ping 
- A network utility that is used to test reachability.
- Measures round-trip time. (PC1 -> PC3 + PC3 -> PC1)
- Uses two messages:
        ICMP Echo Request
		ICMP Echo Reply
![[Pasted image 20250214150128.png]]

 - By default, a ping in Cisco IOS sends 5 ICMP echo requests, and then you should get 5 ICMP echo replies back, and the default size of each ping is 100 bytes.
 - The periods indicates a failed ping, and the exclamation points indicates a successful ping.
 -  In last it gives the round trip time with min/avg/max time in ms.
 - Here first ping fails, because of ARP PC1 didn't know the destination of MAC addresses, so it had to use ARP, and in that time the first ping fails.
 - Lets look at the ARP table.
 - **Command for Windows, macOS, and Linux to see ARP table** `arp -a`, However is cisco IOS it's `show arp`  from privileged exec mode.
 ![[Pasted image 20250214150845.png]]
 ![[Pasted image 20250214150939.png]]
 - **Command to View mac address in CISCO switches** `show mac address-table`
 ![[Pasted image 20250214151420.png]]
The **MAC address table** (also called the **CAM table**) is used by a Cisco switch to map MAC addresses to specific switch ports. It helps in forwarding frames within a LAN.

The **ARP table** (Address Resolution Protocol table) maps IP addresses to MAC addresses. It is maintained by network devices (routers and Layer 3 switches) to facilitate communication between different subnets.

- **MAC Address Table:** Layer 2, stores MAC-to-port mappings, used for switching frames.
- **ARP Table:** Layer 3, stores IP-to-MAC mappings, used for routing packets.

The dynamic mac address are removed  from the mac address table after 5 minutes. if the switch doesn't get any traffic from the MAC address for 5 minutes, it will remove the entry from table. However you can manually remove any MAC address from the table.

- To clear all mac address which get learned dynamically 
```
clear mac address-table dynamic
```
- To clear any specific mac address use.
```
clear mac address-table dynamic address <mac-address>
```
- To clear any specific mac address by interface.
```
clear mac address-table dynamic interface <interface-id>
```

#### Ethernet Frame Review
In this packet capture a 36-byte ping has sent with command
`ping 192.168.1.3 size 36`
![[Pasted image 20250214152750.png]]

**the minimum payload size for an ethernet frame is 46 bytes**, and As the ping size was 36bytes so there should be 10 bytes padding, these zeros are hexadecimal zeros, each hexadecimal digit is 4 bits, so 2 digits equal 8 bits, or 1 byte, hence there are 20 zeros for 10 bytes.

