# Reverse lookup zone and PTR Records

## Overview
A reverse lookup zone allows DNS to resolves IP addresses back to hostnames. While forward lookup zones resolve Hostnames to IP addresses, reverse lookup zones perform the exact opposite function. This function is mostly used when analyzing logs, monitoring systems, or troubleshooting network activity where only IP addresses are visible.

In Active Directory, reverse lookup zones are considered a best practice because they allow admins and applications to easily identify which host corresponds to a given IP address.

## Objectives
- Understand the purpose of reserve lookup zones
- Understand PTR records
- Create a reverse lookup zone for my network
- Verify that IP addresses can be resolved to hostnames

## Environment
DC01:
- Windows server domain controller
- DNS Server role
DC02:
- Windows Server domain controller
- DNS Server role

Network: 192.168.56.0/24

## Implementation
#### Understanding reverse lookup zones
Reverse lookup zones resolve IP addresses to hostnames. This is the opposite of forward lookup zones, which resolves hostnames to IP addrsses.

Example of reverse lookup:
- 192.168.56.100 -> klarstroem.local.testuser

When installing the DNS server role to an Windows server, by default a reverse lookup zone is not created. Microsoft recommonds and says that best practice to is have have a reverse lookup some and have PTR records for all internal resources we manage. Reverse lookups are performed using PTR records.

#### Creating the reverse lookup zone
Before we can create PTR records, we then need a reverse lookup zone to store them in. We therefore need to create the reverse lookup zone first, and we do so in the DNS Manager.

In server manager there is already a default *Reverse Lookup Zones* forder, we need to create the zone itself by right clicking on the folder and choose *New Zone*:

![New reverse lookup Zone](screenshots/newreversezone.png)

Steps in the New Zone Wizard:

- **Choosing Zone Type:** first we're asked to select the type of zone we want to create. I have selected **Primary Zone** and **Store in Active Directory**. When we store a zone in AD it becomes an Active Directory Integrated zone, meaning:
  - The zone data is stored inside the AD database
  - Replication happens through AD replication
  - All DNS servers on domain controllers can update the zone
  - Even though I selected Primary, the result is actually multi-master DNS
![Zone type](screenshots/zonetype.png)

- **Data replication:** next is to choose how we want zone data to be replicated, since we only host a single domain then i'll select *To all DNS servers running on domain controllers in this domain: klarstroem.local*. This means that The zone data is going to be replicated to DC01 and DC02 in our environment because both are DNS servers in the same domain. If we later create sub-domains, then zone data will still be replicated to those aswell because they're still in the same domain.  
![Where to replicate data](screenshots/datareplication.png)

- **IPv4 or IPv6:** we need to choose whether we want to resolve IPv4 or IPv6 addresses, I choose to create a IPv4 Reverse lookup Zone.

- **Network ID:** we have to provide the network ID: 192.168.56.X. The systems knows we're running a /24 network, and therefore the last octet is grayed out because it's the host portion and will be populated by hosts in the network when they register DNS data.

  Example of PTR records that will appear inside the zone:
  -  10 -> dc01.klarstroem.local
  - Full example: 10.56.168.192.in-addr.arpa -> dc01.klarstroem.local. DNS resolves names from right to left, and therefore it address looks a bit strange.
  - The zone itself represents the network: 192.168.56.0/24 so the reverse zone becomes 56.168.192.in-addr.arpa. Inside the zone we then store PTR records such as 10 -> dc01.klarstroem.local.
![Network ID](screenshots/networkid.png)

  
- **Dynamic updates:** It asks us whether we want to allow dynamic updates to enable DNS client computers to register and dynamically update their resource records with the DNS servers, this is the recommended approch my Microsoft. The firste, Secure dynamic updates option, simply means that only clients that are domain-joined and thereby are authenticated are having their DNS data updated in the zones.
![Dynamic updates](screenshots/dynamicupdates.png)

#### Dynamic PTR records registration
PTR records can be created manually, but can as mentioned above also be created automatically "dynamically", this is only possible if the following conditions are met:
- Condition 1: A reverse lookup zone exists "We just created one above"
- Condition 2: Dynamic DNS updates are allowed "We allowed Dynamic updates in the last step of the zone creation"

When a domain joined computer registers its A record dynamically, it attempts to also register the PTR record for its IP address, again this will only happen if the two above conditions are met.

To make it crystal clear, dynamic DNS updates are of course directly related to DHCP. When a client recieves an IP address from a DHCP server, it attempts to register its DNS records, this happen for both A records and PTR records automatically. If the client later recieves an new IP address, it updates the records.
- Old PTR record is automatically removed.
- New PTR record is automatically created.

Who actually updates these DNS information? DNS updates can be enabled and handled by the client ifself, but since DHCP if the one assigning IP addresses it is best practice to configure DHCP to handle this. DHCP keeps track, when lease expires or changes, DHCP updates or removes the records.

## Verification
To verify the zone was created successfully and that PTR records can resolve IP addresses to hostnames, i'm going to create a PTR record manually, and test resolution using nslookup. 

Because the reverse lookup zone was created after the forward lookup records already existed for dc01 and dc02, PTR records were not automatically generated for existing recources. DNS only creates PTR records when a new record is registered or when dynamic DNS registration occurs. Existing records should therefore be added manually.

**Manually creating PTR record for DC01:**
In the reverse lookup zone -> right click -> New Pointer (PTR):
![PTR for dc01](screenshots/ptrdc01.png)

- Host IP address: I simply added 10 since the full static address for dc01 is 192.168.56.10
- Host name: you can manually write the hostname, but it is best practice to choose the corresponding recourse in the browse menu, and locate its A record in the forward lookup zone, it will automatically fill the field with dc01.klarstroem.local

In later module, when I join clients to the domain I will also show the automatic creation of A, AAAA, and PTR records.

## Results
The reverse lookup zone successfully allows IP addresses to be resolved to hostnames. PTR records stored in the zone enable admins and systems/ applications to identify hhosts when only IP addresses are known.

**Testing using nslookup**
After we have successfully created the PTR record, I opened command promt and tested the record:
![Testing](screenshots/nslookuptest.png)

## Lessons Learned
This lab demonstrated how reverse lookup zones complement forward lookup zones by enabling IP to hostnames resolution. Revers DNS is particularly useful when analyzing logs, monitoring systems, or troubleshooting network activity.
