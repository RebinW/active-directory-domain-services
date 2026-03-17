# Group Creation and Design

## Overview  
This lab describes the design and implementation of security groups in Active Directory. Groups are used to manage access to resources in a structured way. Instead of assigning permissions directly to users, permissions are assigned to groups, and users are added to those groups.

## Objectives  
- Design a simple and scalable group structure
- Create security groups for access control
- Apply the principle of least privilege
- Avoid direct permission assignment to users
- Focus on one department for demonstration purposes

Scope for this lab:
The implementation focuses on the Trading department. The goal is to demonstrate the concept of group-based access control, not to build a full enterprise structure.

## Environment  

## Implementation  
#### Step 1. Organizational structure for groups
Before creating groups, a clear structure is structure is good practice to ensure scalability and ease of management.

In a previous lab I created several top-level OUs including **02_Groups**, This groups is going to be used to store all the required security groups within the environment.

Within this OU, additional child OUs are created to reflect business departments. This keeps groups organized and aligned with the organizational structure.

In this case, the following child OUs are created:
- HR
- IT
- FINANCE
- Trading
- Risk_Management

This approch ensures:
- Seperation of groups from users
- clear structure by department
- easier management and delegation

#### Step 2. Group design strategy
Groups are designed based on access requirements, not individuals.

Instead of creating groups for specific users, groups instead represent roles or levels of access within a department.

We're going to concentrate on the Trading department, and for the Trading department the following groups are defined:
- Trading_Read
- Trading_Write

#### Step 3. Group creation.
We going to create the groups within: 02_Groups -> Trading

Each Group is configured as:
- Type: Security
- Scope: Global

Groupe score determines where the group can be used and what it can contain.

**Global (used in this lab)**
- Members: users from the same domain
- Usage: can be assigned permissions within the domain and beyond
- Typical use: role-based groups

**Domain local**
- Members: users and from the same domain and trusted domains
- Usage: used to assign permissions to resources in its own domain
- Typical use: resource-based, such as access to a shared folder

**Universal**
- Members: users and groups from multiple domains
- Usage: can be used across domains
- Typical use: multi-domain environments

The reason I chose Glocal scope is because:
- all users are within a single domain
- it simplefies group management

I chose *security groups* because they are used to assign permissions to resources, *Distribution* on the other hand is used for exchange email purposes.

The naming of the groups uses the format -> <Department>_<AccessLevel>, therefore:
- Trading_Read
- Trading Write

![Trading security group](screenshots/securitygroup1.png)
![Trading security group](screenshots/securitygroup2.png)

#### Step 5. Preparation for access control
At this point, the groups do not yet enforce any permissions.

Their purpose is to act as containers for users. In later stages, these groups will be assigned permissions to resources such as shared folders.

## Verification
After creating the groups, I checked that they were placed in the correct location under 02_Groups -> Trading. I also confirmed that both groups were created as Security groups with Glocal scope. The naming was reviewed to make sure it followed the format I decided on, and I verified that the groups appear correctly in Users and Computers.

## Results
At this point, I now have a simple but structured set of groups for the Trading department. The groups reflect different access levels and are ready to be used when assigning users and later when configuring permissions on shared resources.

## Lessons Learned
This step made it clear why groups are used instead of assigning permissions directly to users. It is much easier to manage access when everything is handled through groups. I also saw how important it is to keep a clean structure in AD, especially when seperating users and groups.

## Next steps

