# Domain-Level Group Policy Overview

## Overview
When an Active Directory domain is created, two default Group Policy Objects are automatically generated. These policies provide the basic security configuration for the domain environment. They are called the **Default Domain Policy** and the **Default Domain controller Policy.**

The Default domain policy typically contains domain-wide security settings that apply to all users in the domain. These settings include password requirements, account lookout rules, and Kerberos authentication settings. Because these settings affect the entire domain, they are usually managed at the domain level instead of inside individual Organizational Units.

The Default Domain Controller Policy applies specifically to domain controllers. Domain controllers are the most critical systems in an AD environment because they store the directory database and handle authentication requests. For that reason, this policy focuses on security settings such as auditing and user rights assignments.

In many organizations they avoid making too many changes directly inside the default policies. Instead they create additional Group Policy Objects and link them to OUs.

The purpose of this lab is to show these default policies, udnerstand where they are located, and review the types of security settings that are configured at the domain level.

## Objectives
- Understand the role of domain-level Group Policies in AD
- Locate the default policies that are automatically created
- Explore domain-wide settings

## Environment

## Implementation

## Verification

## Results

## Lessons Learned
