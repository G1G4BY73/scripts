###  Class C network
![[Pasted image 20250214212438.png]]
### Class B network
![[Pasted image 20250214212507.png]]

![[Pasted image 20250214212535.png]]

### Class C
![[Pasted image 20250214212602.png]]
### Class B
![[Pasted image 20250214212642.png]]
### Class A network
![[Pasted image 20250214212726.png]]

## Configuring a CISCO router with IP addresses
![[Pasted image 20250214212817.png]]

>entering privileged exec  mode
```
en
```
Command to check the status of each interfaces on the device, as well as their IP address.
```
show ip interface brif
```
![[Pasted image 20250215132943.png]]
- **administratively down** : interface has been disabled with the `shutdown` command
- This is default Status of Cisco router interfaces.
- Cisco switch interface are NOT administratively down by default
> Notice in our diagram, even though gigabit ethernet 0/0, 0/1, 0/2 on this router are connected to switches, but the interface remain administratively down because the `shoutdown` command is applied to them by default.

- Status column referred to the Layer 1 status of the interface, this is the Layer 2 status. Because the interface are down at Layer 1, Layer 2 can not operate.
## Lets configure 
configuring the gigabit Ethernet 0/0 interface first.

1. Enter global config mode by configure terminal
```
conf t
```
2. To configure a interface enter interface configure mode.
```
inteface gigabitethernet 0/0
```
other way you can do this.
![[Pasted image 20250215134233.png]]
3. Now we are in interface configuration mode. lets set the IP address. that is done by command IP address.
```
ip address <ip-address> <net-mask>
```
4. Now you have to enter the command `no shutdown` which will enable the interface.
```
no shutdown
```
![[Pasted image 20250215134814.png]]
> you can use `do` in front of command to use a privilege exec command from global configuration mode.

- Lets configure IP for 2 interface
![[Pasted image 20250215135054.png]]

- Configuring the 3rd interface.
![[Pasted image 20250215135130.png]]

### Other command to show interfaces.
this command shows lot of information for each interface.
```
show interfaces 
```
For any specific interface details.
```
show intefaces g0/0
```
> BIA is the actual physical address of the interface. however you can actually configure a different MAC address in the CLI

interface description
>interface description are optional but very helpful in identifying the purpose of each interface. or any note.
```
show inteface description
```
Lets configure description for any interface.
go to global configure mode and then interface config mode.
```
int g0/0
```
```
description ## to SW1 ##
```
![[Pasted image 20250215140226.png]]

 