# Forward lookup zones

## Overview  
Forward lookup zones are used to resolve hostnames to IP addresses. In Active Directory, these zones store DNS records that allow clients and server to locate resources and services in the network.

## Objectives  
- Explain the purpose of forward lookup zones
- Examine the zones created in the AD environment
- Identify common DNS records stored in these zones

## Environment  
Same environment as in the previous lab

## Implementation  

#### What are forward lookup zones  
These zones hold record types that resolve host names to IP addresses. Althrough many different record types exist in these zones common for all of them is that they map hostnames to IP addresses.

Most common record types:
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
This zone is considered the primary zone for the domain. It stores records for hosts and services within the domain

**The _msdcs.klarstroem.local zone:**  
This zone supports Active Directory infrastructure. It stores records used to locate domain controllers and other directory services. Many of the records inside this zone are SRV records that allows clients to locate domain controllers. I have chosen to dedicate a seperate lab to exactly this because of its importance within Active Directory.

## Verification

## Results

## Lessons Learned

## Next steps

