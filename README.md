# Windows Active Directory installation process for Home Labs
A guide to make installing the AD environment on Windows Server as easy as possible.

The main purpose of the project is to facilitate to anyone that needs to build an homelab with Active Directory implementation.
As the domain will be on premise, the virtualization software chosen was VirtualBox, but the vm configuration settings can be also used for others virtualization softwares such as QEMU, HyperV or VMWare.

The virtual machine configurations are:

    -  Windows Server 2022 ISO
    -  4 GB RAM
    -  2 Processor Cores
    -  128 MB Video Memory(This part is optional as you can define the right amount of it)
    -  50 GB Hard Disk
    -  1 Network Interface Card configured on NAT and 1 Network Interface Card configured with Internal Network

OBS: The reason for the NAT adapter choice was not on purpose as you can also use an Bridge adapter to establish internet access, but basically there is a WAN connection and a LAN connection, the chosen network adapter for external access can be configured to establish connection outside the domain with NAT feature, it will be the last topic covered here.

## Why you need an AD domain home lab

The Microsoft Windows Active Directory work as a database manager which contains all the actives used in the organization, Servers, Routers and even Users. Each element stored is treated as a object, to organize this objects we have Organizational Units, think them as files in a directory and as the permissions in this directory, the Group Policy is applied on them but these policies are not applied to Containers, these units grouped are organized in domains, those domain when grouped turn into a tree, the tree establish between the domains a trust relationship so they can share resources among them.

The Forest is an up level of hierarchy as it group many trees, sharing a directory schema and the resources in a trust relationship.

Take as example the company SENTINEL, we have in this company the tree "yellowsentinel.com" which has also other domains like "hr.yellowsentinel.com", "it.yellowsentinel.com" and sharing among them a trust relationship, but this actual corporation is in Fortaleza-Ceará, but now the company is starting to grow an need another builing in Hong Kong, now our structure is the forest SENTINEL, we have another tree call "bluesentinel.com" and others domains like "hr.bluesentinel.com", "it.bluesentinel.com" sharing among them another trust relationship, the forest now defines the directory schema for all the domains, as they are from different trees the relationship make sure the resources can be globally accessed.

Many attacks like Kerberoasting, Privilege escalation, can occur with bad active directory implementation so the best way is to get your hands dirty and analyse the Indicators Of Compromise, Logs and also Processes, wait for others implementations.

## Installation Process

With the ISO inserted on the VM, the installation process starts.

You first select the language, as my ISO was download with the main language as English, the selected languague will be English, click Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/uRFc0R3.png" alt="ADinstall1">
</p>

There are two options for the Desktop environment: Just Command Line or the Graphical Interface version, here the selected one will be the Standard Evaluation and Desktop Experience, click Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/zt55Mu2.png" alt="ADinstall2">
</p>

You can then click to accept the terms and select the Custom Installation, click Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/lpbAS7e.png" alt="ADinstall4">
</p>

Just click Next to install the Operating System into our 50 GB Hard Disk, click Next to install the ISO.

<p align="center">
  <img width="600" src="https://i.imgur.com/XQb0I40.png" alt="ADinstall5">
</p>


## Configuration Process

The first screen that appears is asking you for you main Administrator password, you just type one that you can remember, although is for a Home Lab the Windows Server has it's own Password Policy, so passwords very 
guessable are not accepted, the one chosen here was "P@ssword1", click in Finish.

<p align="center">
  <img width="600" src="https://i.imgur.com/4ZY2OyC.png" alt="ADinstall6">
</p>

The welcome locked screen needs the key combination [Ctrl+Alt+Delete] to unlock the screen.

<p align="center">
  <img width="600" src="https://i.imgur.com/uksHfLU.png" alt="ADinstall7">
</p>

If you try to type this combination manually it will activate your machine combination so in the Virtual Box menu located in the top of the
screen you can select the option at [Input -> Keyboard -> Insert Ctrl+Alt+Delete], mine will appear as PT_br but the options are in the same
spot in both languages, then just type the admin credentials to Log in.

<p align="center">
  <img width="600" src="https://i.imgur.com/YHlaEIR.png" alt="ADinstall8">
</p>

Our domain will need few things before the ad installation process start, first we are going to identify the network cards and then select a hostname for our server, to identify the network cards we can do with cmd by typing [ipconfig] to show both the WAN and LAN network adapters.

<p align="center">
  <img width="600" src="https://i.imgur.com/s7v36jO.png" alt="ADinstall9">
</p>

To make it easy to identify, the NAT reserved IP for Virtual Box is commonly with the start "10.0.2.", also some routers identify with the connection-specific DNS as it's own hostname, in my case "dlinkrouter" so this Ethernet 2 is our NAT card. Not only this can help but also as a standard configuration the Windows give one Automatic Private IP Address(APIPA) to each interface that doesn't receive an IP from some DHCP server, just for guarantee the LAN communication, so the Ethernet is our internal network card.

To rename then, go to Network Connections, click with the right button over then and then click on "Rename", click in "OK".

<p align="center">
  <img width="600" src="https://i.imgur.com/SQ7euWv.png" alt="ADinstall10">
</p>

Now we go to the Internal Network properties and change the IPv4 address, here we can choose the "192.168.0.1", a 256 hosts mask as "255.255.255.0", as we are the internal network gateway we don't need one and for the DNS address we can set the Loopback IP or "127.0.0.1" and then click OK.

<p align="center">
  <img width="600" src="https://i.imgur.com/Aj3BLhi.png" alt="ADinstall11">
</p>

After it, we rename our server, to make it easy to identify you can select any name as "DC", "DOMAINCONTROLLER","DOMAIN" etc.

<p align="center">
  <img width="600" src="https://i.imgur.com/48b4HvY.png" alt="ADinstall12">
</p>

Wait for the server reboot, then we can start the AD installation and configuration, open the "Server Manager" window, click on the "Add roles and features", this options is used to add some features to our server such as the Active Directory, DNS, DHCP server, Remote Access and many others.

<p align="center">
  <img width="600" src="https://i.imgur.com/5ELu64w.png" alt="ADinstall16">
</p>

Click Next and then it will show the actual host and then the existing IP addresses, click Next again.

<p align="center">
  <img width="600" src="https://i.imgur.com/quxZjnU.png" alt="ADinstall17">
</p>

Now select the "Active Directory Domain Services" and "DHCP Server", it will appear an window to confirm the features added, just click "Add Features" and click Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/utMppLA.png" alt="ADinstall19">
</p>

In the next windows some informations are presented to the user but you can just click Next as they appears.

<p align="center">
  <img width="600" src="https://i.imgur.com/1rIzTuj.png" alt="ADinstall20">
</p>

If the configuration succeeds it will appear an yellow sign under the top flag sign above the add features menu, if you click it, the drop down menu shows the details about the features installed and asking for the configuration.

<p align="center">
  <img width="600" src="https://i.imgur.com/YT54qHj.png" alt="ADinstall22">
</p>

Click on the "Promote this server to a domain controller" option, the windows opened is the Deployment Configuration window, used to configure the domain. As we don't have other domain or other forest to connect to we click on "Add a new forest" and specify the domain information, the domain here can be anything you like but for facilitate our domain here will be called "sentinels.com", the delegation is chosen by the user it accepts "local", "org", "com", then click Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/Hm3PeVJ.png" alt="ADdc2">
</p>

The Forest Funcional Level and Domain Functional level can be both Windows Server 2016, in a enterprise environment this setting is used to establish communication between the moderns and legacy server used in others domains. Type the "Directory Services Restore Mode" password, this option is used in case of failure in the domain or the restores some nodes, then click Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/Zr9mF0k.png" alt="ADdc3">
</p>

As we don't have a DNS delegated zone we click Next in the DNS Options.

<p align="center">
  <img width="600" src="https://i.imgur.com/BXJFwG3.png" alt="ADdc4">
</p>

Click Next on the Additional Options.

<p align="center">
  <img width="600" src="https://i.imgur.com/F8hvkJt.png" alt="ADdc5">
</p>

In the Paths, are show the main files and where each one are stored, click Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/5FUfjsB.png" alt="ADdc6">
</p>

The Review will show you the options chosen, here we have also the option "View Script", this option will show you the powershell script that can be used to replicate this configuration in others domains, if you want to you can save anywhere as a ".ps1" file, then in next installation you can just run the script to apply the same configurations used here, then click Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/0yl30zq.png" alt="ADdc7">
</p>

After the prerequisites check, click Install.

<p align="center">
  <img width="600" src="https://i.imgur.com/Bfv4dC5.png" alt="ADdc8">
</p>

After the installation the server will reboot again, but now, you can see that the Log On screen show not only the username but also our domain, the standard pattern for the usernames show in the lock screen is: DOMAIN\User.

Insert the credentials configured before to enter.

<p align="center">
  <img width="600" src="https://i.imgur.com/7RKZEYc.png" alt="ADdc9">
</p>

We have our AD domain configured and operating.

## DHCP Server Configuration

As we have our AD Domain configured and operating, we are going now to configure the DHCP server, as we already have our DHCP server feature installed, we can now just configured it, go to the "Server Manager" window and click in "Tools" and then click on "DHCP Server".

Click on the Domain shown to expand the options, right click on the IPv4 and select "New Scope", here we are going to configure the scope used here, click Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/ipe0off.png" alt="ADdhcp1">
</p>

Select the Scope Name, here we can name the server as the range it supports, you can call as you want but if you want to manage others scopes make it easy to remember, click Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/xdDkBmB.png" alt="ADdhcp2">
</p>

At the IP Address Range you select the start and end for the range, if you configure an A(10.0.0.) or B(172.16.0.) class IP address but you do want to use and class C mask, set the Subnet Mask down below by the address or CIDR notation in the "Length" option, click Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/DGDezII.png" alt="ADdhcp3">
</p>

If you do want to configure any exclusion range, set here or just leave it empty and click Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/7TBB7is.png" alt="ADdhcp4">
</p>

The lease duration it's from you, again, in a enterprise environment, some places such as a Coffee Shop don't need an 8 days of lease time as the client only came by 3 hours, drink a coffee and then leave. The IP will be reserved to the specific host that claimed it first until the DHCP server make other offer after the lease time passes, click on Next.

Although this server is configured for a home lab project, some concepts are revised to make a parallel to a corporate environment.

<p align="center">
  <img width="600" src="https://i.imgur.com/AwfdkgY.png" alt="ADdhcp5">
</p>

The Router or Default Gateway is where we configure our internal IP server address in our case will be "192.168.0.1", after type, click on Add and then Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/clhvw2b.png" alt="ADdhcp6">
</p>

For the DNS server make sure the Parent Domain is the one you delegate as the root forest and the IP address does match with the server internal IP, if not, adjust it by renaming the domain and typing the correct IP address and clicking on "Add", if there is also other IP, click on "Remove", after the adjustments click Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/eV1H1Z8.png" alt="ADdhcp7">
</p>

Then select the "Yes, I want to activate this scope now" option, click Next and close.

<p align="center">
  <img width="600" src="https://i.imgur.com/fkFSNT5.png" alt="ADdhcp8">
</p>

We now have the DHCP server configure but we need to authorize it, to do this right click our domain under DHCP and click "Authorize", to make sure is active click in the IPv4 and in the right window will appear our scope.

<p align="center">
  <img width="600" src="https://i.imgur.com/ZtAq2ou.png" alt="ADdhcp9">
</p>

If anything goes wrong, you can delete the scope and restart the process.

## Configuring and Activating our user account

Now that we have a domain, we need an user account to the others machines.

To create a user search for "Active Directory Users and Computers" and then click on it, click on our domain and then will be listed our Organizational Units, they work as directories for the AD environment, you can create one just for the users by right click in the domain and then click in "New" and "Organizational Unit" or create a new user under the already created "Users" Organizational Unit, click in "New" and after it click in "User".

<p align="center">
  <img width="600" src="https://i.imgur.com/yXlFfw0.png" alt="ADuser1">
</p>

It will open the new user creation window, insert the First and Last names and then the user logon. An commonly standards of users in some environments are: first name letter + last name all in lowercase, so for the John Wick user will have the "jwick" user logon name, click Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/NKmxl08.png" alt="ADuser2">
</p>

Select the password and again the password policy takes the lead to make sure you follow every requirement, you can simulate an real environment by leaving selected the "User must change password at next logon" so at the first log on, the user will be prompted to modify the password or just select the "Password never expires" option to leave the password as you configure.

<p align="center">
  <img width="600" src="https://i.imgur.com/EyvZGEY.png" alt="ADuser3">
</p>

After it, if you want to test you can right click the Organizational Unit "Users" again and click in "Find" to open the "Find Users, Contacts and Groups" window, just type the username in "Name" and click in "Find Now" and the "Search results" show the user selected.

<p align="center">
  <img width="600" src="https://i.imgur.com/qXyUsNW.png" alt="ADuser4">
</p>

For the user virtual machine we can use the same specifications as the server only changing the amount of network interface cards and the ISO used, as we are gonna need to install a desktop operational system such as Windows 10, 11 or use a preconfigured OVA image, the network interface card used is configured here as "Internal Network".

After the user machine boots up, check the IP address and make sure it corresponds to the DHCP server ip range configured.

<p align="center">
  <img width="600" src="https://i.imgur.com/MfleIZA.png" alt="ADuser6">
</p>

As we can see the machine not get only the IP Address but also the Gateway and the DNS suffix as our domain.

Now, to insert the user in the domain we can search for the System information or just press [Windows+PauseBreak], again, mine will appear as Portuguese but the spot is the same for both languages, click in "Change Settings" or "Alterar Configurações" and then click in "Change" or "Alterar" and then you click on "Member of" ou "Membro de" and type the domain you configured, here the domain typed was the one we configure as "sentinels.com" and click OK.

<p align="center">
  <img width="600" src="https://i.imgur.com/J0nF0EP.png" alt="ADuser7">
</p>

After it, you will be prompted for the username and password we created early, just type and click in OK.

<p align="center">
  <img width="600" src="https://i.imgur.com/rG1InST.png" alt="ADuser8">
</p>

 A welcome screen pop up and the machine will reboot.

<p align="center">
  <img width="600" src="https://i.imgur.com/0dvlbuD.png" alt="ADuser9">
</p>

Again, to enter the Domain Log On screen insert via the Virtual Keyboard the combination of keystrokes [Ctrl+Alt+Delete].

<p align="center">
  <img width="600" src="https://i.imgur.com/BsmWFrA.png" alt="ADuser10">
</p>

Select the option "Other User" ou "Outro Usuario" and then type the username and password.

<p align="center">
  <img width="600" src="https://i.imgur.com/njUSOqV.png" alt="ADuser11">
</p>

To check if you are really inside the domain, press [Windows+PauseBreak] or go to System information, it should appear the hostname and the domain you entered.

<p align="center">
  <img width="600" src="https://i.imgur.com/L1jVUiI.png" alt="ADuser12">
</p>

From now you have an totally functional AD server with DHCP and a user test to start the experiments in your home lab.

## NAT Configuration for internet access

If you do want to also access internet inside the domain, you can add the NAT feature to your server.

Go to Server Manager and then in Add Features and roles, select the option "Remote Access" and then click Next in the options and then Install.

<p align="center">
  <img width="600" src="https://i.imgur.com/J0aq6lJ.png" alt="ADextras1">
</p>

After the install, click in the "Tools" options and then in "Routing and Remote Access". Right click the domain hostname and then click in "Configure and Enable Routing and Remote Access", click in Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/ZoseHF6.png" alt="ADextras3">
</p>

In the Configuration screen select the Network Adddress Translation option and then click Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/gAKRYic.png" alt="ADextras4">
</p>

Click in the internet interface, if it doesn't appear, close the window and then restart the NAT configuration clicking in the "Routing and Remote Acess" again, select the Network interface that show the WAN IP address, in our case the WAN IP is the NAT "10.0.2.15", click Next.

<p align="center">
  <img width="600" src="https://i.imgur.com/BA0kGyJ.png" alt="ADextras5">
</p>

After it, click in Finish and then it will configure the NAT feature.

<p align="center">
  <img width="600" src="https://i.imgur.com/crHLHrC.png" alt="ADextras6">
</p>

The domain icon now appears with an green arrow pointing up, this means that the NAT configuration was successfull.

<p align="center">
  <img width="600" src="https://i.imgur.com/ZFyt7bm.png" alt="ADextras7">
</p>

Now, if we go back to our machine and then test our external connection via the ping command we can now access internet internally in our environment.

<p align="center">
  <img width="600" src="https://i.imgur.com/njiWuAb.png" alt="ADextras8">
</p>

As we can see the icmp packets are successfully reaching the facebook server, so the NAT routing works pretty well.



Now, that the Active Directory is fully configured is up to you to implement in the homelab and then start installing the utilities to train security controls in Microsoft Windows environments.
One great reference to this project's concept was the youtuber [Josh Madakor].

[Josh Madakor]: https://www.youtube.com/watch?v=MHsI8hJmggI&t=32s

