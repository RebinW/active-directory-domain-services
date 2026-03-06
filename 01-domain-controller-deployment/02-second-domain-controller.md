# Deploying a second domain controller

## Overview
This lab documents the deployment of a second domain controller to our AD environment. The goal is to provide redundancy, enable replication between DC'S, and improve the resiliance of authentication and DNS services.

I the previous lab [domain controller deployment](https://github.com/RebinW/active-directory-domain-services/blob/main/01-domain-controller-deployment/01-domain-controller-deployment.md) I went through each step on how to install AD DS and how to promote the server to be a domain controller. In this module I wont document the same step, but instead focus on what to be aware of when deploying a second domain controller to an existing domain.

## Objectives
1. Understanding the importance of multiple domain controllers.
2. Configure a second domain controller in an existing domain.
3. Verify Active Directory replication.
4. Verify DNS replication between domain controllers.

## Environment
- **Domain:** KlarStroem.local
- **Network:** 192.168.56.0/24
- **Servers:**
  - DC01 - Primary domain controller
  - DC02 - Additional domain controller  
- **Technologies:**
  - VirtualBox
  - Windows Server 2019
  - Active Directory Domain Services
  - DNS server

**Insert diagram here later**

## Implementation
#### Step 1: Installing Windows Server and configuring network settings
As mentioned previously, we're not going through setting up the VM in detail because the specs are the same as DC01. After Windows Server have been installed I then configured the network settings:

- **Host-Only adapter:**
  - Static IP address: 192.168.56.11/24
  - DNS: 192.168.56.10 "DC01"
  - Default gateway: none
- **NAT Adapter:**
  - Uses DHCP for network configuration

**Important:** Before installing AD DS and promotion of the server, the server must join the domain "klarstorem.local" to establish a trust relationship with Active Directory. This trust relationship will allow authentication with the existing domain controller "DC01", which is required during promotion because our new domain controller must replicate the Active Directory database from an existing domain controller. For this reason I configured the DNS to point to DC01, so that the server can find the existing domain controller and join the domain.

#### Step 2: Domain join DC02
We made sure that DNS settings points to DC01, this ensures that the server can locate the existing domain controller.

There are several ways to join a device to a domain, I used the following path; Server manager -> Local Server -> Domain -> Change.

In the section **Member of** we chose domain and type the name: klarstroem.local, it will then ask us to authenticate.

![Joining the server to klarstroem.local](screenshots/domainjoin.png)

We have now succesfully joined the server to the domain, and the server has become a **Member Server**. If we look under Users and Computers, DC01 will then appear as a client under computers. In the next step after promotion we'll see that DC02 will no longer be a member server but instead a domain controller, therefore it will no longer appear under **Computers** but instead under **Domain controllers**.

Most importantly, we have now build the trust relationship so that replication can happen succesfully during promotion.

#### Step 3: Installing AD DS and promoting DC02 to a domain controller
After we have successfully joined the server to the domain, we then start with installing the AD DS role on the server "exactly the same as in the previous lab".

After installation, in Server Manger will notice a yellow triangle wit an exclamation mark under notifications, here it will asks us to promote the server to become an domain controller.

From here, we have to select the deployment operation. Out of the three options we're presented with we are of course going to choose **Add a domain controller to an existing domain**. 

![deployment operation](screenshots/deploymentoperation.png)

After it will ask us to include Global Gatalog and Install DNS Server, and after we continue under **Additional Options** we can specify replication options. We only have DC01 so we can just choose to replicate from any domain controller, but we in a real organization they would chose to replicate from the domain controller closets for performance reasons.

![replication](screenshots/replication.png)

## Verification
