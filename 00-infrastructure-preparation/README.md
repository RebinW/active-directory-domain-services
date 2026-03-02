# Preparation and configuration of the domain controller  
In this lab, i'm going to go through the setup of installing and configuring the domain controller.  

## VM design decisions

I'm going to use VirtualBox to run the virtual environment and install my VM "Windowsm Server", these are the following specs:
- VM Name: **DC01**
- OS Edition: **Windows Server 2022**
- Memory: **4096 MB "4 GB"**
- CPU: **2 CPU'S**
- Harddisk: **80 GB VDI "VirtualBox Disk Image" Dynamic**
  - Dynamic: This means the virtual disk consumes physical storage on the host "my PC" on as data is writtren inside the OS. The disk expands automatically up to the configured maximum size "80 GB". In other words, the VM does not reserve all 80 GB on the host PC from the beginning.

**IMPORTANT**  
NOTE: My default *Unattended installation is checked*, I disabled the option during VM creation. The unattended setup automatically applies predefined identity and systenm configurations such as user accounts, hostname, and DNS suffix "Domain name". For my project I went with a manual installation to ensure a clean baseline server and full admin control over all configurations. This allows the server identity, networking configuration, and later Active Directory domain setup to be defined as part of the learning and documentation.

EXAMPLE OF UNATTENDED INSTALLATION "NOT CHOSEN"  
![Unattended](screenshots/unattended.png)

User name and password section:
- VirtualBox creates a local user automatically during Windows installation
- It does not log us in as the built-in Admin account
- It creates a seperate local account (vboxuser) and password we enter

OS Installation options section: 
- Hostname: The name of the server
- Domain name: This field does not refer to an Active Directory domain. It specifies a temporary DNS suffix automatically applied during automated Wiondows setup. This suffix is used to create a network identity for the VM during installation, forexample DC01.myguest.virtualbox.org. Since this project requires creating an Active Directory domain manually during domain controller promotion, this automatic DNS suffix was avoided to maintain a clean server config before AD DS deployment. 

## Rename Server
When we created the VM and named it DC01, we then only named the VM inside of VirtualBox. The name then only belongs to the hypervisor, not to Windows.

VirtualBox VM name:
- Used only by VirtualBox to identify the VM on my host PC

Windows computer name:
- Used by the operating system and the network

**Why renaming must happen before installing AD DS?**  
When we promote a server to a domain controller:
- The hostname becomes permanently embedded in Active Directory.
- DNS records are created using that name.
- Changing it later is complex

To change the hostname of the server:
- Server Manager - Local Server - Computer name - Change:
![Change hostname](hostname)
## Network configuration

