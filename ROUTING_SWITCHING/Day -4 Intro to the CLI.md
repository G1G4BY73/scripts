> Introduction to IOS CLI

Cisco IOS is the Operation system  used on cisco devices like windows on a pc or macOS on a iMac.
> Cisco's IOS is not related to apples IOS used in iPhones.

#### What is a CLI ? 
- Command line interface
- The interface you use to configure Cisco Devices 
![[Pasted image 20250113200855.png]]

GUI - Graphical User interface . for cisco's ASDM which can be used to configure Cisco's Firewalls.
![[Pasted image 20250113201009.png]]

However we will not use GUI in this course, and most network engineer prefers to use the CLI over GUI.

> How exactly do you connect to a cisco device to configure it with the CLI ? 

- There are couple of methods, but I'm going to talk about connecting via console port.
- Connecting to a device via console port typically involves bringing your laptop to the device and connecting to the console port of the device. 
- There are also methods to connect to a device remotely, but we'll cover that later
- In any case, when you first configure a device you have to connect to the console port.
- This below is the image of a cisco catalyst switch and notice the two console ports.
![[Pasted image 20250113201614.png]]

- You can connect to either of these, but let's say we're going to connect to the RJ45 port. you going to need the proper cable.
- below is the kind of cable you will use notice one end has an RJ45 connector, just like Ethernet UTP cable we looked at. the other end is DB9 connector. However most laptops these days no longer having a serial port to plug the cable into, so you'll probably  need an adaptor like this, to connect to a USB port on your laptop.
![[Pasted image 20250113202133.png]]

- The actual name of the cable on the left is a `rollover cable`.
- The name is similar to crossover cable, but it's different than an ethernet UTP crossover cable.
- Like in an Ethernet cable, there are 8 pins on  each end that are used, however  they connect differently.
![[Pasted image 20250113202409.png]]

- So, Once you've connected your computer to the device, how do you actually access the CLI ? 
- Well, you need to use a terminal emulator, this one here, PuTTy, is a popular choice.
- select Serial here and you find the COM port in device manager and give speed of device which it operates. and press open
- You should be connected to the CLI.  you  should be able to connect to Default setting but you can view and edit default settings for the serial connection.
![[Pasted image 20250113202948.png]]

*Default settings : 9600 speed, 8 data bits, 1 stop bits, no parity, no flow control.*

Once you connected to the device, you will be greeted with a screen like this. since this is the first time booting up the device, I am asked if I would like to enter the initial configuration dialog, and I answer no.
Then I pressed the enter key to get started, and I'm now free to type commands into the CLI

##### User EXEC Mode
- When you first enter the CLI, you will by default be in what's called `user EXEC mode `
- user EXEC mode is indicated by the 'greater than sign' next to hostname of the device.
- Notice the default hostname to this device is Router.
- All devices have a hostname, and for a Cisco router a default hostname is Router.
- user EXEC mode is very limited, users  can look at some things, but can't make any changes to the configuration.
- Usually you don' t do anything in this mode. you also might hear is called `user mode`.
![[Pasted image 20250113203907.png]]

##### Privileged EXEC Mode
- if you `enable` command in user EXEC mode you will be placed in privileged EXEC mode.
- Privileged EXEC mode provides complete access to view the device's configuration, restart the device, etc.
- this in not the mode in which you actually change the configuration, however you can change the time on the device, and also save the current configuration file, among many other things.
![[Pasted image 20250113223737.png]]

*This comparison of the commands available in user EXEC mode and privileged mode*
![[Pasted image 20250113224137.png]]
> these commands are available on packet, there are many thing which packet tracer doesn't support, so if you check on real devices you would probably see even more commands available here.
![[Pasted image 20250113224314.png]]

#### Global Configuration Mode.
 Lets Make Some changes to routers configuration
- to do so we need to enter `global configuration mode`. The command to enter global configuration mode is `configure termainl`
- When in global configuration mode, `config` is inserted after the hostname
> once again you don't have to type the whole word 
![[Pasted image 20250113224703.png]]

- Now we don't want just anyone to be able to make changes to the network configuration, or even be able to just look at the configuration because that could be security risk.
- We can protect the privileged exec mode with a password, so if a user enter the `enable command` from user EXEC mode, they are asked for the password.
- That is done with the `enable password` in global configuration mode.
![[Pasted image 20250113225219.png]]<cr> means there are no further options

Lets set a password for our device.

![[Pasted image 20250113225506.png]]

##### Lets take a look at the configuration file itself to check it.

First I want to Explain the two configuration files kept on the device.
As I said there are two separate configuration files kept on the device at once.

![[Pasted image 20250113225916.png]]

- Use the `show running-config` command in global configuration  mode to view the running configuration.
- There is lots of data in the file by default, so I only include the output up to the command we entered.
![[Pasted image 20250113230137.png]]

How about the startup-config ?
- you can view it with the `show startup-config` command,
- however as you can see, at this point of time it just displays the message `startup-config is not present`
![[Pasted image 20250113230518.png]]
- This is because we haven't saved the running configuration yet, so every time we restart  it will load a default configuration, not the startup-configuration.
- Now lets save the configuration file
- There are actually 3 way to save the running configuration file. to make it the startup configuration.
- all these commands are executed from privileged EXEC mode.
![[Pasted image 20250113230650.png]]
- Now use `show startup-config` this time you will see the startup configuration file.

- Now you can see the enable password right here, in plain text. This is a security risk, because anyone who simply takes a glance at this password knows how to get into privileged exec mode, and then global configuration mode to make changes. to the device.
- So, Lets level up the security a bit. you can do so with the `service password-encryption` command in global configuration mode.
- Notice I used the short version of configure terminal, conf t, to enter global configuration mode. remember that all of these commands have shortcuts.
- The service password-encryption command will encrypt all passwords in jumble of numbers and letters, so that they cannot be easily read.
- if I enter the show running-config command again, notice that the password CCNA  has been changed.
![[Pasted image 20250113231558.png]]
> No `7` indicates the type of encryption used to encrypt the password.

- No 7 means it is using Cisco's proprietary encryption algorithm, from the service password-encryption commands
- While using the `service password-encryption` command is more secure than not using it, it is not very security any password cracker may crack this encryption.
- there is a more secure enable password that can be used on cisco devices, with a tougher type of encryption.
- The more secure method is to use the `enable secret` command, instead the `enable password`. command
- This time I used the password of Cisco, with a upper case C. Once again this is case-sensitive. then is use the show running configuration once more.
- However, since I was in global configuration mode, I typed `do` in front of the command. This is a convenient trick that allows you to execute privileged EXEC mode commands like show running-config or show startup-config in other configuration levels.
![[Pasted image 20250113232647.png]]
> Also notice that the enable password command remains, it isn't replaced. However, if both the enable secret and enable password commands are configured, the enable password will be ignored. only the enable secret will be valid.
> One final note, the `service password-encryption` command have No effect on the enable password secret command.

###### How to cancel, or delete a command that you entered.
That is done by typing `no` in front of the command. for example if I type `no` in front of the service password encryption command, future password will no longer encrypted. However, as you can see here. password which are already encrypted will NOT be decrypted.
![[Pasted image 20250113233316.png]]

![[Pasted image 20250113233358.png]]
###### Summary for the day.
#### Modes review
![[Pasted image 20250113233457.png]]
#### Commands review
![[Pasted image 20250113233531.png]]
![[Pasted image 20250113233556.png]]
![[Pasted image 20250113233626.png]]
![[Pasted image 20250113233640.png]]

## Summary
- A rollover cable is used to connect to the RJ45 console port on a cisco device.
