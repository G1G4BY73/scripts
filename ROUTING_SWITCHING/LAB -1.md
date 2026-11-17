
> Lets configure the router.

*Configure the hostname for router*

-> Enter privilege exec mode.
`enable`
->Enter global configuration mode.
`config t`
->Change the hostname
`hostname R1`

*Configuring the hostname for Switch*

-> Enter privilege exec mode.
`en`
-> Enter Global configuration mode.
`conf t`
->Change the hostname
`hostname SW1`


*Applying password on router*

-> Enter to global config and then enter the below command, This password is for privilege ex  mode. 
`enable password CCNA`

 
*Check the password in running config*

-> with privilege exec mode 
`show running config`

*Protecting password with Encryption*

-> this must be done from global config model.
`service password-encryption`

-> Now if you check this in running config you won't see password in clear text.
> Don't forgot to change to priv. exec mode or use do with command in global config mode.


*To configure a more secure password use secret*

-> global config
`enable secret cisco`

> If we check in running config. you will a hash instead of CISCO


*Save the running configuration to the startup configuration*

>there are three ways to do this

-> privilege exec mode
`write`
`write memory`
`copy running-config startup-config`


***All these mentioned commands are same for Router and Switches**

