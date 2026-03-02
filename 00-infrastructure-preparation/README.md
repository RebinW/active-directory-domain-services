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
- Windows setup runs without asking us anything

We want to avoid this because:
- We want clean server
- We want control over the admin account during setup

## Network configuration
Server renaming
Network explanation
Why DNS points to itself
