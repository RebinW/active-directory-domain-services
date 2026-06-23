# DNS architecture and design

## Overview  
DNS maps hostnames to IP-addresses in the network and is an important componenet in the Active Directory environment. Active Directory relies on DNS to locate domain controllers and services. When a client/PC tries to authenticate, it queries DNS for service records (SRV Records) which identify available domain controllers. Without DNS, domain authentication, service discovery, and many other core Active Directory functions would fail.

This document describes the DNS architecture used in this project and explains the deesign decisions behind the selected deployment model.

## Objectives  
- Explain the role of DNS in an AD environment
- Describe common DNS deployment models
- Document the DNS architecture used in this project
- Give reasoning for choosing Active Directory integrated DNS zone

## Environment  
Domain name: klarstroem.local

DC01:
- Windows Server Domain controller
- DNS Server role

DC02
- Windows Server Domain controlelr
- DNS server role

The environment uses an **Active Directory integrated DNS zone** hosted on both domain controllers. DNS runs on each domain controller, and the zone data replicates through Active Directory replication.

## Implementation  

#### Role of DNS in Active Directory  
DNS enables name resolution and service discovery within the network. In Active Directory environments, DNS is required for locating domain controllers and other services.

When a client tries to authenticate, it sends a DNS query requesting a service record, example: _ldap._tcp.dc._msdcs.klarstroem.local

The DNS server then responds with the hostnames of available domain controllers. The client then resolves the hostnames to an IP address and contacts the selected domain controller for authentication.

#### DNS Zones  
DNS Zones are databases containing DNS records for a specific domain.

The primary forward lookup zone in this environment is: **klarstrom.local**, here are some example records types stored in this zone:
  - A records
  - AAAA records
  - SRV recors
  - CNAME records

Active Directory also creates an additional zone

**_msdcs.klarstroem.local**

This zone stores records used by Active Directory to locate domain controllers and support replication.

All records within the forward lookup zone revolves hostnames to IP-addresses regardless of the record type. There is also a **reverse lookup zone**, records inside this zone do the opposite, they resolve IP-addresses to hostnames instead.

#### DNS Record types  
- A records: Resolves hostnames to IPv4 addresses
- AAAA records: Resolves hostnames to IPv6 addresses
- SRV records: Identifies servers that provide specific services such as LDAP or Kerberos
- PTR records: Used in reverse lookup zones to map IP-addresses to hostnames
- NS records: Identifies which DNS servers are authoriative for a zone
- CNAME records: Creates an alias that points to another hostname
  - Example: files.klarstroem.local -> CNAME -> server01.klarstroem.local.
- SOA record: Defines authority information for the zone and includes replication and refresh settings.

#### DNS Deployment models  
DNS zones can be deployed using several models.

**Primary and secondary zones:**  
A primary zone stores the writable copy of the DNS data base on a single DNS server. Secondary zones hold read only copies of the zone and recieve updates through zone transfers.

This model is used mostly if the server is a dedicated DNS server not running domain services. The model creates a single write point and requires manual management of zone transfers.

**Stub zones:**  
Stub zones contains only a small database of records from another zone. It stores information about the authoritative DNS servers for that zone and helps DNS servers locate those servers.

Stub zones are used when a company has several domains. When a clients needs to access a resource in the other domain, the DNS server in its own domain does not hold records from other domains services and resources. The stub zone created in its own domain therefore always hold records that would point to the other domains authoritative DNS server, that would then help the client locate the resource in that domain.

An alternative to a Stub is something called **conditional forwarders**. This does the exact same thing as a Stub zone, but instead of holding a subset of the other domains DNS data, it then simply instead points to the other domains authoritative DNS server. It is much simpler to configure, but if the other domains DNS server for any reason changes IP-Address it will not be able to send queries because it does not update automatically, were as a Stub does exactly that.

**Active Directory integrated zones**  
Active Directory integrated zones store DNS zone data inside the AD database. Replication happens automatically through Active Directory replication. 

This model uses multi master updates, meaning multiple DNS servers can accept changes to the zone. This is exactly the set-up we are running, and this is also the recommended approch by Microsoft. The reason for choosing this setup is:

- Replication: DNS data replicates automatically between domain controllers through Active Directory replication.
- High availability: Both DC01 and DC02 host the DNS zone and can process DNS queries which gives us redundancy.
- Multi master updates: Both DNS servers are writable and can accept dynamic updates.

## Verification  
DNS zones were automatically created during domain controller promotion:
- Forward lookup zone: klarstroem.local
- AD service discovery zone: _msdcs.klarstroem.local

Both zones appear on DC01 and DC02, confirming that the zone is replicated through Active Directory. This lab is just to give an introduction to DNS, in future labs I will create zones and records to verify replication between domain controllers.

## Results  
The project and environment uses an Active Directory integrated DNS zone hosted on both domain controllers. DNS data replicates automatically between DC01 and DC02, providing redundancy and ensurinmg dynamic updates.

## Lessons Learned  
DNS is a crucial and foundational component of Active Directory infrastructure. Correct DNS design ensures reliable service discovery and authentication.

Active Directory integrated zones simplefies DNS management compared to other deployment options. It eliminates the need for manual zone transfers and enables multi master replication between domain controllers.

Later DNS labs will go into more depth on DNS records, reverse lookup zones, and especially service discovery using SRV records. 
