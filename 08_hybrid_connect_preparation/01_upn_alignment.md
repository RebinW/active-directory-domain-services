# UPN Alignment before synchronization

## Overview
Before configuring Entra ID Connect, I reviewed the user logon formats in both the on-prem AD and in Entra ID. I noticed that the domain suffix used on-premise was different from the one used in the cloud tenant. Since synchronization depends on matching identities correctly, this needed to be handled before moving forward with hybrid identity.

This lab focuses on aligning the UPN suffix in AD so that it matches the Entra ID tenant domain. Instead of changing users one by one, Powershell was used to update all users in bulk.

## Objectives
- Identify differences between on-prem and cloud UPN formats
- Understand why UPN alignment is required before synchronization
- Add the cloud user UPNs using powershell
- Verify that the new UPN format was applied successfully

## Environment

## Implementation

## Verification

## Results

## Lessons Learned

## Next steps
