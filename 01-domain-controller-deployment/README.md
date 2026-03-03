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



- Forest creation
- Domain naming decision
- Functional level explanation
- Safe Mode password purpose
