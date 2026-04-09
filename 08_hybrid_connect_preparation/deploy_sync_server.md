# Install and configure Microsoft Entra Connect Sync

## Overview
In this lab, I'm going to install and prepare a dedicated Windows Server to be the synchronization server between the on-premise Active Dirctory environment and Microsoft Entra ID.

## Objectives
- Deploy a dedicated Windows Server to be the synchronization server between on-premise Active Directory and Entra ID.
- Install Microsoft Entra Connect Synch
- Configure synchronization between the on-premise Active Directory and the Entra ID Tenant
- Validate that objects created in Active Directory are synchronized to Entra ID
- Verify that password changes performed on-premise are successfully synchronized to Entra ID
- Confirm that synchronized users can authenticate to Microsoft cloud services using on.prem credentials
- Document key configuration decisions, including domain filtering, OU selection, and synchronization settings.
- Troubleshoot sync and permission issues

## Environment
Domain: klarstroem.local  
Entra Tenant: KlarStroem.onmicrosoft.com  
Domain Controllers:
- DC01: 192.168.56.10
- DC02. 192.168.56.11

Synchronization server:
- SYNC01: 192.168.56.12

Network model:
- Host-only adapter for internal communication
- NAT adapter for internet access

## Implementation
**Preparation**
Before installing Microsoft Entra Connect, I installed and configured a decdicated Windows Server 2022 called SYNC01. The server was configured using the same baseline configuration used in previous server deployments in this project "DC01, DC02"

I domain-joined the server to the klarstroem.local Active Directory domain and placed it in the Servers/ Infracstructure OU. Network connectivity was verified and tested to ensure connection between servers and with external internet resources.

DNS settings were configured to point to the domain controllers, and time sync was confirmed to prevent authentication and sync issues.

**Step 1 - Download Entra Connect Sync**
I started the installation from the decidated synchronization server, SYNC01. From that server I signed in to my Entra ID tenant, and from  here I downloaded the installer: 
- Entra ID Blade -> Entra Connect -> Connect sync

I chose connect sync instead of Cloud Sync because I wanted my lab to reflect a moretraditional enterprise setup. Cloud Sync is lighter and easier to deploy, but connect Sync is still the most used in organizations and runs on a dedicated server and need more control. Since the purpose of this lab is to build a realistic on.premise to cloud synchronization environment, Connect sync was the better option. 

Additionally, I used this resource to better understand the differences between the two main options and their capabilities: [Connect sync VS Cloud Sync](https://learn.microsoft.com/en-us/entra/identity/hybrid/common-scenarios)

## Verification

## Results

## Lessons Learned
