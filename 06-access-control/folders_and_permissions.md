# Department folders, Security Groups, NTFS Permissions, and Access Based Enumeration

## Overview
This lab builds on lab 1 by expanding the file share into a structured file storage model within the domain. The goal is to create a realistic enterprise-style folder hierarchy, assign access through security groups, apply NTFS permissions based on least privlege, and enebale Access-Based Enumeration so users only see folders they are allowed access to.

## Objectives
- Create a department-based folder structure inside the shared volume
- Assign NTFS permissions based on group membership
- Apply the principle of least privlege to shared folder access
- Enable Access-Based Enumeration on the shared folder
- Verify that users only access and view folders they are authorized to use

## Environment
Domain: klarstroem.local  
Domain controllers: DC01 &DC02

Shared storage:
- Share name: \DC01\SharedNetworkFolders
- Local path: Z:\Shares\SharedNetworkFolders

## Implementation
#### Step 1. Create the department folder structure
Inside the existing folder path Z:\Shares\SharedNetworkFolders, a separete folder was created the Trading department

Structure: Z:\Shares\SharedNetworkFolders\Trading:
![New Trading folder](screenshots/tradingfolder.png)

This folder represents the Trading department and will be used to demonstrate role-based access using existing groups created earlier

#### Step 2. Use existing Active Directory structure
Instead of creating new groups, the lab uses the already created structure in Active Directory.

Organizational Units:
- OU: 02_Groups
- Child OU: Trading

Existing security groups:
- Trading_Read
- Trading Write

Existing users:
- Mark Nielsen, member of Trading_Read
- Line Hansen, member of Trading_Write

This shows a realistic setup where identity objects are already provisioned and access control is applied afterward

#### Step 2. Understand Default NTFS inheritance
When the Trading folder was created, it automatically inherited permissions from its parent folder: Z:\Shares\SharedNetworkFolders

Inherited entities included:
- CREATOR OWNER
- SYSTEM
- Administrators
- Users

This is expected and normal in NTFS. All newly created folders inherit permissions from their parent unless inheritance is disabled.

I observed that the "Users" group included not only read permissions, but also special permissions that allowed file creation. This goes against what I intended.

#### Step 3. Disable inheritance on the Trading folder
To enforce strict access control, inheritance was disabled on the Trading folder, steps: 
1. Right-click Trading -> Properties -> Security -> Advanced'
2. Click Disable inheritance
3. Select -> Convert inherited permissions into explicit permissions

This ensures that existing groups stayed, but that we're now able to remove or modify groups as wanted.

#### Step 4. Remove broad default groups.
After disabling inheritance, unnecessary and groups were removed.

Removed -> Users (KLARSTROEM\uSERS)

Reason: The "Users" group applies to all domain users and included special permissions such as file creation. Keeping this group would be against the principle of least privilege.

#### Step 5. Apply Role-Based Access using existing groups.
Instead of assigning permissions directly to users, existing security groups were used.

Groups:
- Trading_Read
- Trading_Write

User mapping:
- Mark Nielsen -> Trading_Read
- Line Hansen -> Trading_Write

Permissions asssigned:
- Trading_Read -> Read and Execute
- Trading_Write -> Modify



## Verification

## Results

## Lessons Learned

## Next steps
