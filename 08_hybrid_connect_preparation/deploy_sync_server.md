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

Additionally, I used this resource to better understand the differences between the two main options and their capabilities: [Connect Sync VS Cloud Sync](https://learn.microsoft.com/en-us/entra/identity/hybrid/common-scenarios)

**Step 2 - Running the installer and selecting custom configuration**  
After downloading the installer, I ran Microsoft Entra Connect Sync setup on the SYNC01 server using admin privleges. The installer started by presenting two configuration options: *Express option* and *Customize Option*

![Choosing deployment](screenshots/selectconfigoption.png)

Even though Express in my case would work because of a simple environment with a single forest, I chose the *Customize* optin. This allowed me to manually review and configure each step/ stage of the installation instead of relying on default settings.

Choosing Customize gave me a better understanding and visibility into the configuration process and allowed me to control important settings such as sign in method, directory connection, filering options, and optional features. This lab focuses on understanding synchronization and not just deploy it, then custom configuration option made more sense to me.

**Step 3 - Install required components**  
After selecting the custom configuration option, the installer moved to *Required Components* section. The installer checked the server and confirmed that no existing synchronization service is installed.

Since this was the first time deploying Entra connect Sync in the environment, none of the advanced installation options were selected.

![Required components options](screenshots/requiredcomponents.png)

Still I think it's important to understand these options:
- Sepcify a Custom installation location: The default installation path was used because there was no requirement to change the location.
- Use an existing SQL Server: This option was not required because the installer automatically deploys a local SQL Server Express instance. Thi sdatabase is used to store synchronized identity data, track changes, and maintain synchoronization state between Active Directory and Entra ID.
- Use an existing Service Account: This was not selected because no sync acoount existed yet. The installer later created the required Active Directory account automatically, which is used to read directory objects.
- Specify custom sync groups: This option was not required in this lab because default permissions are sufficient.
- Import sync settings: This optiopn is used when rebuilding or migrating an existing sync server. Since this was my first deployment, there were no previous settings to import.

**Step 4 - Select the user sign-in method**  
At this point I had to choose the user sign-in method, and I went with Password Hash Synchronization. 

![Sign-in options](screenshots/signinoptions.png)

I chose PHS because it is one of the most used methods in hybrid setups. It is also the simplest to maintain compared to other methods that require additional infrastructure. In this setup, password hashes from the on-premise Active Directory are synchronized to Entra ID. The hashed passwords from on-premise are processed again and re-hashed before being stored in the cloud, and user authentication is then handled by Entra ID instead of the domain controller.

I also enabled the option called sigle sing-on. This allows users who are signed in to their domain-joined computers to access cloud services without having to type their password again. This improves the user experince and reduces the number of times users need to enter credentials and use MFA.

**Additional note**: Even though PHS was selected in this step, this does not replace the authentication methods used inside the on-premise environment. Users who sign in to domain-joined computers inside the local network are still authenticated by Active Directory using Kerberos. PHS only affects how authentication works when users access cloud services.

When users access cloud resources, authentication is handled by Entra ID instead of the domain controller. In most modern environments, this authentication rpocess uses protocols such as OpenID Connect for authentication and OAuth 2 for Authorization.

Understanding how these protocols, methods and tokens work is important to me, but this lab focuses only on configuring the sync setup. The detailed authentication flow, including access tokens, and ID token, will have their own dedicated labs.

## Verification

## Results

## Lessons Learned
