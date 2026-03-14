# OU-Level Group Policy implementation

## Overview
Group Policy Objects can be applied at different levels in an Active Directory environment. They can be configured on the local machine, at the site level, at the domain level, or at an Organizational Unit level. This order is described as Local, Site, Domain, and OU.

Policies that are applied further down this order will take priority over the ones applied earlier. This means that policies applied at an OU level can override settings that exist at the domain level. Because of this, most companies apply the most of their policies at the OU level. This allows admins to target specific users, workstations, or servers without affecting the entire domain.

In real organizations Group Policy is well thought off when enforcing security rules and system configurations. The number of available policies and settings is extremely large and organizations often spend a long time designing and managing them.

For this project only a few examples will be configured. The purpose of this lab is not to build a full enterprise policy configuration og setup, but simply to demonstrate how Group Policy Objects are created, configured, and linked to Organizational Units. This lab builds on the OU structure created earlier and demonstrates how policies can be applied to specific parts of the directory structure.

## Objectives
- Understand how GPO can be linked to Organizational Units
- Demonstrate how policies applied at the OU level can override domain policies
- Create example policies targeting users and workstations
  
## Environment

## Implementation

## Verification

## Results

## Lessons Learned

**Note on Hybrid Identity:** The policies configured in this lab apply only to the on-premise environment. When users and devices are later synchronized to Entra ID using Entra Connect, these Group Policy settings are not automatically transferred to the cloud.

On-prem authentication is still handled by the domain controllers, which means these policies will still apply when users sign in to domain-joined computers. Cloud services such as Microsoft 365 use different ways for access control and configuration, such as conditional access and Intune policies.
