# Creating and configuring the client VM

## Overview
In this lab, a client VM is created and configured to prepare it to be domain joined. The goal is to establish a working client device that can communicate with the domain controllers and later be used to test authentication and access control.

The lab includes selecting the correct operating system, allocating recourses, and configuring networking to ensure connectivity within the environment.

## Objectives
- Create a client virtual machine for the lab environment
- Install a supported Windows operating system
- Allocate correct resources
- Configure networking to enable communication with domain controllers
- Prepare the device for formain join

## Environment
The following setup and environment is used:
- Hypervisor: VirtualBox
- Operating systems: Windows 11 Pro
- Domain: klarstroem.local

Virvial machine configuration:
- RAM: 4 GB
- CPR: 2 Processors
- Disk: 50 GB (dynamically allocated VDI)

## Implementation
#### Step 1. Creating the virtual machine
A new virtual machine is created in VirtualBox. During creation the following resources are assigned:
- 4 GB RAM
- 2 CPUs
- 50 GB dynamically allocated disk
  
I downloaded a Windows 11 ISO file, and during installation I made sure to choose Windows 11 **Pro** since other versions do not support domain-join capabilities:
![Windows 11 Pro](screenshots/image.png)

#### Step 2. Configure networking
To ensure communication within the environment, the virtual machine is configured with two network adapters:
- Host-Only adapter: Used for internal communication "LAN"
- NAT adapter: Used to provide internet access "WAN"

I added these two adapter directly in VirtualBox by right clikking on the CLIENT01 -> Settings -> Network:

![Network adapters](screenshots/networkadapters.png)

After the two adapters had been added, I then went and configured the network settings in the VM. 

1. Internal network adapter "Host-Only":
![Host-Only adapter](screenshots/internal.png)

2. Internet adapter "NAT":
   ![NAT Adapter](screenshots/network.png)

   
## Verification

## Results

## Lessons Learned
