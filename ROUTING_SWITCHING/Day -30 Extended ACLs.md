> Topics to Cover

-> Another way to configured Numbered ACLs
-> Edit ACLs
-> Extended numbers and named ACLs

![[Pasted image 20250423145010.png]]


![[Pasted image 20250424000914.png]]

## Advantages of named ACL config mode.

#### Deleting individual Entries 
![[Pasted image 20250424001012.png]]

 How about doing it from a global config mode.
![[Pasted image 20250424001144.png]]

*Hence if you want to edit a ACL u should use name ACL config mode.*


#### Insert  New entries b/w other entries
![[Pasted image 20250424011352.png]]


## Resequencing ACLs
![[Pasted image 20250424011621.png]]


# Extended ACLs
- Extended ACLs functions mostly the same as standard ACLs.
- They can be numbered or named, just like standard ACLs.
  -> Numbered ACLs use the following ranges: 100-199, 2000-2699
- They processed top to bottom just like standard ACLs.
- However, they can match traffic based on more parameters, so they are more precise (and more complex) than standard ACLs.
- We will focus on matching based on these main parameters: Layer 4 protocol/port, source address, and destination address.

![[Pasted image 20250424012446.png]]

![[Pasted image 20250424012705.png]]
![[Pasted image 20250424012754.png]]

![[Pasted image 20250424013014.png]]

![[Pasted image 20250424013132.png]]