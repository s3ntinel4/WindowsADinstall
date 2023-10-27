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

Take as example the company SENTINEL, we have in this company the tree "yellowsentinel.com" which has also other domains like "hr.yellowsentinel.com", "it.yellowsentinel.com" and sharing among them a trust relationship, but this actual corporation is in Fortaleza-Ceará, but now the company is starting to grow an need another builing in Hong Kong, now our structure is the forest SENTINEL and then another tree call "bluesentinel.com" and others domains like "hr.bluesentinel.com", "it.bluesentinel.com" and sharing among them a trust relationship, the forest now has a only directory schema for all the domains.

Many attacks like Kerberoasting, Privilege escalation, can occur with bad active directory implementation so the best way is to get your hands dirty and analyse the Indicators Of Compromise, Logs and also Processes, wait for others implementations.

## Installation Process

With the ISO inserted on the VM, the installation process starts.

You first select the language, as my ISO was download with the main language as English, the selected languague will be English, click Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/f04deefc-cdbd-417e-bd76-9bad81cd2a60" alt="ADinstall1">
</p>

There are two options for the Desktop environment: Just Command Line or the Graphical Interface version, here the selected one will be the Standard Evaluation and Desktop Experience, click Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/645c2d17-fd55-43c1-81fb-40cbab340a39" alt="ADinstall2">
</p>

You can then click to accept the terms and select the Custom Installation, click Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/41923f95-6381-41db-b416-84649619acbb" alt="ADinstall4">
</p>

Just click Next to install the Operating System into our 50 GB Hard Disk, click Next to install the ISO.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/f37c3545-dc2e-4232-bbf1-2e6742f984e3" alt="ADinstall5">
</p>


## Configuration Process

The first screen that appears is asking you for you main Administrator password, you just type one that you can remember, although is for a Home Lab the Windows Server has it's own Password Policy, so passwords very 
guessable are not accepted, the one chosen here was "P@ssword1", click in Finish.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/f5dfdf9e-3770-4b01-99c0-6748a46a63c5" alt="ADinstall6">
</p>

The welcome locked screen needs the key combination [Ctrl+Alt+Delete] to unlock the screen.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/cf16c115-6d85-4aac-b482-d3b55ff8c162" alt="ADinstall7">
</p>

If you try to type this combination manually it will activate your machine combination so in the Virtual Box menu located in the top of the
screen you can select the option at [Input -> Keyboard -> Insert Ctrl+Alt+Delete], mine will appear as PT_br but the options are in the same
spot in both languages, then just type the admin credentials to Log in.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/cb91e81d-bfe6-448d-81ec-799fb59724bc" alt="ADinstall8">
</p>

Our domain will need few things before the ad installation process start, first we are going to identify the network cards and then select a hostname for our server, to identify the network cards we can do with cmd by typing [ipconfig] to show both the WAN and LAN network adapters.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/a0074145-0a29-4fb1-a5b6-abd1aea219b4" alt="ADinstall9">
</p>

To make it easy to identify, the NAT reserved IP for Virtual Box is commonly with the start "10.0.2.", also some routers identify with the connection-specific DNS as it's own hostname, in my case "dlinkrouter" so this Ethernet 2 is our NAT card. Not only this can help but also as a standard configuration the Windows give one Automatic Private IP Address(APIPA) to each interface that doesn't receive an IP from some DHCP server, just for guarantee the LAN communication, so the Ethernet is our internal network card.

To rename then, go to Network Connections, click with the right button over then and then click on "Rename", click in "OK".

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/fcf9723a-8d5e-49c1-8d68-9469a6124f94" alt="ADinstall10">
</p>

Now we go to the Internal Network properties and change the IPv4 address, here we can choose the "192.168.0.1", a 256 hosts mask as "255.255.255.0", as we are the internal network gateway we don't need one and for the DNS address we can set the Loopback IP or "127.0.0.1" and then click OK.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/f280645c-d404-4303-b9ae-fe19fb609e7e" alt="ADinstall11">
</p>

After it, we rename our server, to make it easy to identify you can select any name as "DC", "DOMAINCONTROLLER","DOMAIN" etc.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/190b8bf5-f976-466c-80da-08b7e7c31dbb" alt="ADinstall12">
</p>

Wait for the server reboot, then we can start the AD installation and configuration, open the "Server Manager" window, click on the "Add roles and features", this options is used to add some features to our server such as the Active Directory, DNS, DHCP server, Remote Access and many others.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/13b9f1c2-f120-4369-85e5-3eb95a7cbef4" alt="ADinstall16">
</p>

Click Next and then it will show the actual host and then the existing IP addresses, click Next again.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/07cf56d2-3a1b-4cdc-88f8-90e71c053ca0" alt="ADinstall17">
</p>

Now select the "Active Directory Domain Services" and "DHCP Server", it will appear an window to confirm the features added, just click "Add Features" and click Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/7b724244-3995-40dd-a02c-c4e75d123853" alt="ADinstall19">
</p>

In the next windows some informations are presented to the user but you can just click Next as they appears.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/e56c874d-54d2-4aac-8bfe-88619161d201" alt="ADinstall20">
</p>

If the configuration succeeds it will appear an yellow sign under the top flag sign above the add features menu, if you click it, the drop down menu shows the details about the features installed and asking for the configuration.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/f744a4ab-4554-4785-9150-dc665829f02e" alt="ADinstall22">
</p>

Click on the "Promote this server to a domain controller" option, the windows opened is the Deployment Configuration window, used to configure the domain. As we don't have other domain or other forest to connect to we click on "Add a new forest" and specify the domain information, the domain here can be anything you like but for facilitate our domain here will be called "sentinels.com", the delegation is chosen by the user it accepts "local", "org", "com", then click Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/4ff9ba4e-0589-40a1-97e2-2b8eb400c133" alt="ADdc2">
</p>

The Forest Funcional Level and Domain Functional level can be both Windows Server 2016, in a enterprise environment this setting is used to establish communication between the moderns and legacy server used in others domains. Type the "Directory Services Restore Mode" password, this option is used in case of failure in the domain or the restores some nodes, then click Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/197c02a1-ec43-4098-b4bf-48962f88f18e" alt="ADdc3">
</p>

As we don't have a DNS delegated zone we click Next in the DNS Options.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/689fd440-72a8-4af6-a936-27a35f77af7a" alt="ADdc4">
</p>

Click Next on the Additional Options.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/311bfbce-94ab-496e-8da5-002bd1a22ce4" alt="ADdc5">
</p>

In the Paths, are show the main files and where each one are stored, click Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/34bd09b5-e83c-46c1-ad19-4e2a0ca76884" alt="ADdc6">
</p>

The Review will show you the options chosen, here we have also the option "View Script", this option will show you the powershell script that can be used to replicate this configuration in others domains, if you want to you can save anywhere as a ".ps1" file, then in next installation you can just run the script to apply the same configurations used here, then click Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/e07ae865-5830-4ad4-bd35-bfc84e03d360" alt="ADdc7">
</p>

After the prerequisites check, click Install.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/5e835934-6dd6-440c-8b9c-8377b4ae20fa" alt="ADdc8">
</p>

After the installation the server will reboot again, but now, you can see that the Log On screen show not only the username but also our domain, the standard pattern for the usernames show in the lock screen is: DOMAIN\User.

Insert the credentials configured before to enter.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/f44e4a6c-411a-4aed-847c-be9a6e4eb805" alt="ADdc9">
</p>

We have our AD domain configured and operating.

## DHCP Server Configuration

As we have our AD Domain configured and operating, we are going now to configure the DHCP server, as we already have our DHCP server feature installed, we can now just configured it, go to the "Server Manager" window and click in "Tools" and then click on "DHCP Server".

Click on the Domain shown to expand the options, right click on the IPv4 and select "New Scope", here we are going to configure the scope used here, click Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/3281376d-e90c-4073-9d19-c9b38cf5f795" alt="ADdhcp1">
</p>

Select the Scope Name, here we can name the server as the range it supports, you can call as you want but if you want to manage others scopes make it easy to remember, click Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/fc85f3fe-6420-4eb7-bf5e-6ea1b99432ef" alt="ADdhcp2">
</p>

At the IP Address Range you select the start and end for the range, if you configure an A(10.0.0.) or B(172.16.0.) class IP address but you do want to use and class C mask, set the Subnet Mask down below by the address or CIDR notation in the "Length" option, click Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/e90fdd67-3052-4f3e-9800-0a2e7605eb93" alt="ADdhcp3">
</p>

If you do want to configure any exclusion range, set here or just leave it empty and click Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/10d13a32-4cd5-42aa-ab37-cdd8fb8e095b" alt="ADdhcp4">
</p>

The lease duration it's from you, again, in a enterprise environment, some places such as a Coffee Shop don't need an 8 days of lease time as the client only came by 3 hours, drink a coffee and then leave. The IP will be reserved to the specific host that claimed it first until the DHCP server make other offer after the lease time passes, click on Next.

Although this server is configured for a home lab project, some concepts are revised to make a parallel to a corporate environment.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/a3842396-78b4-4029-af72-7e9b365a71f9" alt="ADdhcp5">
</p>

The Router or Default Gateway is where we configure our internal IP server address in our case will be "192.168.0.1", after type, click on Add and then Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/ff69a5fc-7228-43cf-bb9d-2aaf68758f6b" alt="ADdhcp6">
</p>

For the DNS server make sure the Parent Domain is the one you delegate as the root forest and the IP address does match with the server internal IP, if not, adjust it by renaming the domain and typing the correct IP address and clicking on "Add", if there is also other IP, click on "Remove", after the adjustments click Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/5b342cd1-9266-45cb-aa58-9de40d852366" alt="ADdhcp7">
</p>

Then select the "Yes, I want to activate this scope now" option, click Next and close.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/a4b6c9e0-a712-471b-a934-2bd09029ec3c" alt="ADdhcp8">
</p>

We now have the DHCP server configure but we need to authorize it, to do this right click our domain under DHCP and click "Authorize", to make sure is active click in the IPv4 and in the right window will appear our scope.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/b8938268-3826-46ca-8de3-429544729de6" alt="ADdhcp9">
</p>

If anything goes wrong, you can delete the scope and restart the process.

## Configuring and Activating our user account

Now that we have a domain, we need an user account to the others machines.

To create a user search for "Active Directory Users and Computers" and then click on it, click on our domain and then will be listed our Organizational Units, they work as directories for the AD environment, you can create one just for the users by right click in the domain and then click in "New" and "Organizational Unit" or create a new user under the already created "Users" Organizational Unit, click in "New" and after it click in "User".

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/6d40ab4d-39b7-430d-9f85-cb2ec6cc9fa2" alt="ADuser1">
</p>

It will open the new user creation window, insert the First and Last names and then the user logon. An commonly standards of users in some environments are: first name letter + last name all in lowercase, so for the John Wick user will have the "jwick" user logon name, click Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/ac1b033f-77c1-437d-9175-1d816e4c1512" alt="ADuser2">
</p>

Select the password and again the password policy takes the lead to make sure you follow every requirement, you can simulate an real environment by leaving selected the "User must change password at next logon" so at the first log on, the user will be prompted to modify the password or just select the "Password never expires" option to leave the password as you configure.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/ad54bda6-5474-488d-a7ca-44e24022ce85" alt="ADuser3">
</p>

After it, if you want to test you can right click the Organizational Unit "Users" again and click in "Find" to open the "Find Users, Contacts and Groups" window, just type the username in "Name" and click in "Find Now" and the "Search results" show the user selected.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/8aa7bdf2-41de-4566-a597-70d6be1fee07" alt="ADuser4">
</p>

For the user virtual machine we can use the same specifications as the server only changing the amount of network interface cards and the ISO used, as we are gonna need to install a desktop operational system such as Windows 10, 11 or use a preconfigured OVA image, the network interface card used is configured here as "Internal Network".

After the user machine boots up, check the IP address and make sure it corresponds to the DHCP server ip range configured.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/90bfb011-ff8f-45ec-b49a-a3ae0d375712" alt="ADuser6">
</p>

As we can see the machine not get only the IP Address but also the Gateway and the DNS suffix as our domain.

Now, to insert the user in the domain we can search for the System information or just press [Windows+PauseBreak], again, mine will appear as Portuguese but the spot is the same for both languages, click in "Change Settings" or "Alterar Configurações" and then click in "Change" or "Alterar" and then you click on "Member of" ou "Membro de" and type the domain you configured, here the domain typed was the one we configure as "sentinels.com" and click OK.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/9b38bc48-b32c-4717-9fab-c3c361e798d6" alt="ADuser7">
</p>

After it, you will be prompted for the username and password we created early, just type and click in OK.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/4eb215b2-e065-4288-be59-d8169ef9103a" alt="ADuser8">
</p>

 A welcome screen pop up and the machine will reboot.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/c3585ecf-2a87-495c-bfef-51b2138ab9f9" alt="ADuser9">
</p>

Again, to enter the Domain Log On screen insert via the Virtual Keyboard the combination of keystrokes [Ctrl+Alt+Delete].

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/37e0b7f9-bfed-4d0a-8d8c-c81254991c64" alt="ADuser10">
</p>

Select the option "Other User" ou "Outro Usuario" and then type the username and password.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/64c3ab2c-8807-4ddd-9fb5-272f5de21088" alt="ADuser11">
</p>

To check if you are really inside the domain, press [Windows+PauseBreak] or go to System information, it should appear the hostname and the domain you entered.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/55602c79-0f95-4c0a-abfb-540ac98a7e37" alt="ADuser12">
</p>

From now you have an totally functional AD server with DHCP and a user test to start the experiments in your home lab.

## NAT Configuration for internet access

If you do want to also access internet inside the domain, you can add the NAT feature to your server.

Go to Server Manager and then in Add Features and roles, select the option "Remote Access" and then click Next in the options and then Install.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/be3a8093-f5fa-4920-84f0-1daf024df190" alt="ADextras1">
</p>

After the install, click in the "Tools" options and then in "Routing and Remote Access". Right click the domain hostname and then click in "Configure and Enable Routing and Remote Access", click in Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/e679d1d8-bcd9-4a94-a36c-29bc4bee42fd" alt="ADextras3">
</p>

In the Configuration screen select the Network Adddress Translation option and then click Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/c8ccfce6-4bfe-4a0e-ba1a-690d66bc08d7" alt="ADextras4">
</p>

Click in the internet interface, if it doesn't appear, close the window and then restart the NAT configuration clicking in the "Routing and Remote Acess" again, select the Network interface that show the WAN IP address, in our case the WAN IP is the NAT "10.0.2.15", click Next.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/e08e0eb3-3112-4307-851a-fdc980ca6d4e" alt="ADextras5">
</p>

After it, click in Finish and then it will configure the NAT feature.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/d12d375c-186c-4811-8839-64fa4ece3dd3" alt="ADextras6">
</p>

The domain icon now appears with an green arrow pointing up, this means that the NAT configuration was successfull.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/383dd491-4de4-4676-beec-7cb5626cd5ba" alt="ADextras7">
</p>

Now, if we go back to our machine and then test our external connection via the ping command we can now access internet internally in our environment.

<p align="center">
  <img width="600" src="https://github.com/s3ntinel4/WindowsADinstall/assets/89402071/8bcd1cc5-64bb-473c-aa21-2d123e79fca8" alt="ADextras8">
</p>





Now, that the Active Directory is fully configured is up to you to implement in the homelab and then start installing the utilities to train security controls in Microsoft Windows environments.
One great reference to this project's concept was the youtuber [Josh Madakor].

[Josh Madakor]: https://www.youtube.com/watch?v=MHsI8hJmggI&t=32s

