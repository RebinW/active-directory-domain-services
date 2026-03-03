## Installing the AD DS role.
Installing AD DS role: 
- Server Manager -> Add roles and features.
  - Installation type: Role-based or feature-based installation.
  - Server selection:  DC01.
  - Server Roles: Active Directory Domain Services.
  - Features: Server Manager will automatically apply and auto select the features that are   required to run AD DS.
 
![Installing AD DS](screenshots/addsinstall.png)

We do not have to check the role **DNS Server** from the menu at this point because during promotion "next step", Windows will automatically install and configure DNS because its required for a domain controller.

## Promote to Domain Controller.

We've installed AD DS, now we need to promote the server to be a domain controller.

Inside Server Manager a yellow triangle will appear saying; *Configuration required for Active Directory Domain Services at DC01* we then press Promote this server to a domain controller:
![Promote the server](screenshots/promoteserver.png)

Inside the configuration wizzard there are mainly two very important sections we should be aware of.  

1. The Deployment Configuration:
   - Here the important part is to select the correct deployment operation. We haven't created a forrest priviously and therefore do not have a domain yet, so we chose **add a new forrest** and name our root domain: klarstroem.local:
![Creating new forest](screenshots/deploymentconfiguration.png)

This creates:
  - A new forest
  - A new domain
  - A new DNS namespace
   

2. The Domain Controller options:
I'd like to explain the different options here:
   - **Functional level:** The minimum Windows Server version allowed for domain controller in this forest, meaning all domain controllers must run Windows Server 2016 or newer.
   - **DNS Server:** It is automatically selected because Active Directory requires DNS
   - **Global Catalog:** Allows efficient authentication and forest-wide object searches. It stores nessesary information from other domains and a full copy of its own domain.
   - **Read-Only Domain Controller:** Used in branch offices where security is limited. The branch office wants:
     - Faster logins locally
     - Reduced WAN traffic
     - No full writable DC in this location
       
      Therefore they can deploy a RODC while head office in another country fully handles       the writable domain controllers.
   - **Directory Services Restore Mode:** This is a recovery password for maintenance and disaster recovery.
