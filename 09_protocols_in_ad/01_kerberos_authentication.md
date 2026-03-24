# Kerberos authentication and ticket lifecycle analysis in AD

## Overview
This lab shows how Kerberos authentication works in Active Directory domain environments. The goal is to observe how a domain-joined client locates domain controllers using DNS SRV records, authenticates using Kerberos, recieves a Ticket Granting Ticket "TGT", and later requests service tickets when accessing network resources.

The lab focuses on understanding the authentication flow rather than performing configuration. We're going to take a closer look at the role of DNS in locating domain controller, the use of password-derived encryption during authentication, and the lifecycle of Kerberos tickets stored on the client PC.

## Objectives
- Understand how domain-joined clients locate domain controllers using DNS SRV records
- Understand how Kerberos authentication begins after domain controller discovery
- Verify that a TGT is issued after successfull authentication
- Take a closer look on how service tickets are generated when accessing resources
- Analyze Kerberos-related security events on the DC

## Environment
Domain: klarstroem.local

Servers:
- DC01 Domain controller, DNS Server, Kerberos Key Distribution Center "KDC"
- DC02: Domain controller, DNS Server

Client:
- CLIENT01: Domain-joined Windows PC

## Implementation



## Verification

## Results

## Lessons Learned

## Next steps
