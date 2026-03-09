# DNS architecture and fundamentals

## Overview  
This lab explains the DNS architecture used in the Active Directory environment and the design decisions behind the chosen deployment model, for that same reason a short explanation of other options will be explained.

## Objectives  
- Explain the role of DNS in Active Directory
- Describe common DNS deployment models
- Document the DNS design used in this lab

## Environment  
- Two windows server both promoted to domain controllers
  - Primary: 192.168.56.10
  - Secondary: 192.168.56.11
- DNS server role is also installed on both servers
- AD domain: klarstroem.local

The environment uses an **Active Directory integrated DNS zone** hosted on both domain controllers. DNS runs on each domain controllers, and the zone data replicates through Active Directory replication.

## Implementation  
Before we start going through the different DNS deployment options, I'd like to go through some fundamental concepts first.  

- DNS Server: A DNS server is the service running on a machine. It answer DNS queries and resolves hostnames to IP addresses and vice versa.
- DNS Zone: A DNS zone is a data base of DNS records
- DNS Records examples:
  - A records: Resolves hostnames to IPv4 addresses
  - AAAA records: Resolves hostnames to IPv6 addresses
  - SRV records: Locates services that provide a specific service, commeon services in AD
    - LDAP
    - Kerberos
    - Global Gatelog
    - Kerberos password change
  - PTR records: 
  - NS records: Identifies which DNS servers are authoriative for a zone
  - CNAME records: Creates an alias that points to another domain name
    - Example: files.klarstroem.local -> CNAME -> server01.klarstroem.local. Basically this allows multiple names to point to the same server.
  



## Verification

## Results

## Lessons Learned

## Next steps
