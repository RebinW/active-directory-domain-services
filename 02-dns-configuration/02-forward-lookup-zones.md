# Forward lookup zones

## Overview  
Forward lookup zones are used to resolve hostnames to IP addresses. In Active Directory, these zones store DNS records that allow clients and servers to locate resources and services in the network.

## Objectives  
- Explain the purpose of forward lookup zones
- Examine the zones created in the AD environment
- Identify common DNS records stored in these zones

## Environment  
Same environment as in the previous lab

## Implementation  

#### What are forward lookup zones  
These zones hold record types that resolve hostnames to IP addresses. Althrough many different record types exist in these zones common for most of them is that they map hostnames to IP addresses.

At first I had a hard time distinguishing the two forward lookup zones from each other, reason being mainly because SRV records exists in both. I learned that the answer different questions. klarstroem.local answers questions like: 
  - which servers provide LDAP for this domain
  - which servers provide Kerberos authentication

The _msdcs zone one the other hand is used for Active Directory infrastructure discovery. It answers questions like:
  - Which machines are domain controllers?
  - which servers are global catelog seervers
  - which DC own a particular directory object
  - Which DC should replication use

So the difference is **service discovery** vs **infrastructure discovery**

Common record types in forward lookup zones are:
- A records
- AAAA records
- SRV records
- CNAME records
- NS records



#### Examining the zones and records created in Active Directory  
We're able to find the DNS zones by opening DNS manager: Server manager -> Tools -> DNS  

![DNS Zones](screenshots/dnsmanager.png)

In the above picture, we see the *forward lookup zone* folder/ container. Inside the container we see the two forward lookup zones *klarstroem.local* and *_msdcs.klarstroem.local*.


**The klarstroem.local zone:**  
This zone is the domain namespace, and is considered the primary DNS zone so it represents the actual domain. Records inside the zone describe hosts in the domain and services provided by the domain


![klarstroem.local zone](screenshots/primaryzone.png)

On the above picture we see some of the different records in this zone. We've got several record types that maps our dc01 and dc02 domain controllers to IPv4 and IPv6 addresses. 

There are also additional folders inside of this domain:
- _sites folder: Used when the company has multiple Active Directory sites, meaning domain controllers in other networks/ locations.
  - Example: _ldap._tcp.siteA._sites.dc._msds.klarstroem.local
- _tcp folder: contains SRV records for services that use the TCP protocol which is most services.
- _udp folder: Contains SRV records for services that use UDP protocol. Some Kerberos and DNS services uses UDP for performance.
- DomainDnsZones folder: This ensures replication of DNS zones to all DNS servers in the domain.
- ForestDnsZones: This ensures replication of DNS data to all DNS servers in the entire forest.
- same as parent folder files: This means that these files belongs to root of the zone meaning klarstroem.local.



**The _msdcs.klarstroem.local zone:**  
This zone supports Active Directory infrastructure. It stores records used to locate domain controllers and other directory services. Many of the records inside this zone are SRV records that allows clients to locate domain controllers. I have chosen to dedicate a seperate lab to exactly this because of its importance within Active Directory.

![msdcs.klarstroem.local zone](screenshots/secondaryzone.png)

#### How records are created in the forward lookup zones  
When recources join the domain, DNS records are dynamically registered in the primary domain zone. Infrastructure records required for locating domain controller are registered in the _msdcs zone by domain domain controllers and Active Directory services.

Forexample, if a server joins the domain that server will first be registered in the primary zone (klarstroem.local) and typical records such as A and AAAA records will be created. Later when we promote that server to a domain controller then SRV records will automatically be created in the _msdcs zone.

## Verification

## Results

## Lessons Learned

## Next steps

