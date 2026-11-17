### IPv4 Classes
![[Pasted image 20250303183121.png]]

![[Pasted image 20250303183142.png]]

![[Pasted image 20250321182912.png]]

![[Pasted image 20250321183100.png]]


##### Can we assign a /30 subnet to a network where 0 usable IPs are available
![[Pasted image 20250321183332.png]]

- Normally this would be a problem, because it leaves no usable addresses after subtracting the network and broadcast address but for point to point network like this, a dedicated connection like this between two routers, there is actually no need for4 a network address or a broadcast address. so we can break the rule in this case. and assign two addresses in this network to our routers, note that, if you try this configuration on a cisco router, you'll get a warning like this.

![[Pasted image 20250321183724.png]]

![[Pasted image 20250321183836.png]]

![[Pasted image 20250321184102.png]]


## Question 1 
![[Pasted image 20250321184246.png]]

You have given a segment of 192.168.1.0/24, Divide the network segment into four subnets that can accommodate the number of host required.

> 192.168.1.0/25

Total IP required 45 host + 2 broadcast & network = 47 IP each subnet so, 47 * 4 = 188 Total IP required

![[Pasted image 20250321184925.png]]

![[Pasted image 20250321185131.png]]

![[Pasted image 20250321185743.png]]

![[Pasted image 20250321190017.png]]

![[Pasted image 20250321190120.png]]

![[Pasted image 20250321190238.png]]

![[Pasted image 20250321190545.png]]

## Question 2

![[Pasted image 20250321190657.png]]

Subnets are as follows.

192.168.255.0/27
192.168.255.32/27
192.168.255.64/27
192.168.255.96/27
192.168.255.122/27

> like this if we do we have to make total 8 subnets.

So if we take a subnet of /27 which means we are taking 3 borrow bits from host bits for subnet but if borrow 3 bit we can make 8 subnets but we want only 5 so what can we do.

![[Pasted image 20250322172442.png]]

![[Pasted image 20250322172540.png]]

we can also add  3 more subnets if require in this segment 
![[Pasted image 20250322172635.png]]

## Question 
![[Pasted image 20250322172905.png]]
## 2. 
![[Pasted image 20250322173056.png]]

	## SOLUTI0N

![[Pasted image 20250322173232.png]]


> converting all hosts bit to zero

![[Pasted image 20250322173339.png]]


# Subnetting CLASS B networks
![[Pasted image 20250322173511.png]]

## Question
![[Pasted image 20250322173849.png]]

![[Pasted image 20250322173905.png]]

 ![[Pasted image 20250322173915.png]]![[Pasted image 20250322173926.png]]![[Pasted image 20250322173952.png]]![[Pasted image 20250322174005.png]]
![[Pasted image 20250322174017.png]]
![[Pasted image 20250322174029.png]]
![[Pasted image 20250322174043.png]]
![[Pasted image 20250322174101.png]]

![[Pasted image 20250322174419.png]]

![[Pasted image 20250322174441.png]]

![[Pasted image 20250322174518.png]]

![[Pasted image 20250322174534.png]]

 ![[Pasted image 20250322174613.png]]
![[Pasted image 20250322174630.png]]

![[Pasted image 20250322174656.png]]
![[Pasted image 20250322174715.png]]

![[Pasted image 20250322174820.png]]


## Question
![[Pasted image 20250322175007.png]]

![[Pasted image 20250322175018.png]]

![[Pasted image 20250322175027.png]]

![[Pasted image 20250322175038.png]]

![[Pasted image 20250322175051.png]]


##### ***Part - 3***
## Answers
## 1. 
![[Pasted image 20250322175930.png]]

## 2.
![[Pasted image 20250322180253.png]]

## 3.
Since we are looking for the broadcast address not the network address. convert all the host bits to 1.
![[Pasted image 20250322180428.png]]
![[Pasted image 20250322180442.png]]


## 4. 
![[Pasted image 20250322180535.png]]

## 5.
![[Pasted image 20250322180623.png]]

# Subnetting CLASS A Networks
![[Pasted image 20250322180651.png]]

## Questions
![[Pasted image 20250322180731.png]]

![[Pasted image 20250322180915.png]]

![[Pasted image 20250322180952.png]]

![[Pasted image 20250322181011.png]]
![[Pasted image 20250322181254.png]]


# VLSM - Variable Length Subnet Masks
Until now, we have practiced subnetting used FLSM (Fixed Length Subnet mask)
- This means that all the subnet use the same prefix length (i.e. Subnetting a class C network into 4 using /26)
- VLSM is the process of creating subnet of different sizes to make your use of network addresses more efficient.
- VLSM is more complicated then FLSM, but it's easy if you follow the steps correctly.

### we have to use only one network segment to assign IPs of all 5 subnets
![[Pasted image 20250322182109.png]]

We can do this with VLSM not with FLSM

### Steps.
![[Pasted image 20250322182604.png]]

![[Pasted image 20250322182644.png]]

###### For Tokyo LAN A
- We have to give a subnet of /25 to make 110 hosts possible.
![[Pasted image 20250322183058.png]]
![[Pasted image 20250322182913.png]]

###### For Toronto LAN B
192.168.1.127 = Broadcast address of Tokyo LAN A
So
192.168.1.128  is the network address of Toronto LAN B ?? what prefix length to use ? ?
>HOST required = 45 so /26 will work
![[Pasted image 20250322183807.png]]

###### For Toronto LAN A
192.168.1.191 = Broadcast address of Toronto LAN B
So
192.168.1.192 is the network address of Toronto LAN A ?? what is the prefix now
> HOST required = 29 so /27 will work

###### For Tokyo LAN B
192.168.1.223 = broadcast address of Toronto LAN A
So 
192.68.1.224 = network address of Tokyo LAN B ?? prefix length 
> HOST Required =14 so /28 will work

![[Pasted image 20250322184502.png]]

###### For Point to point connection 
192.168.1.239 = broadcast address of Tokyo LAN B
So
192.168.1.240 is the network address of point to point connection 
> Host required = 2  we can use /31 but better we use /30 as we have 15 more IP available
![[Pasted image 20250322184810.png]]

![[Pasted image 20250322184822.png]]


> For practice 

![[Pasted image 20250322184931.png]]

