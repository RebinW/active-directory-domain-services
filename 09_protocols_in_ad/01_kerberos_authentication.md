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
#### Step 1 - Client startup and domain controller discovery
When the client computer starts, it recieves an IP and DNS configuration normally from DHCP, but in my case I have set static values. The DNS server assigned to the client is the domain controller.

When the user logs in using a domain account, the client must first locate the available domain controllers before the authentication process can begin.

The clienmt PC sends a DNS SRV query to locate the domain controllers providing LDAP services. The query searches for domain controllers registered under the LDAP service in the domain. The SRV query used by the client:
- _ldap._tcp.dc._msdcs.klarstroem.local
- _service_protocol.dc_msdcs.domainName

Each part of this query:
- _ldap: specifies the requested **service**. in this case, LDAP, which is used to communicate with Active Directory
- _tcp: specifies the protocol used for communication. AD uses TCP for LDAP communication
- dc: specifies that the client is seaching for domain controllers
- _msdcs: represents Microsoft Domain Controller Services. this namespace stores service location records for domain controllers
- klarstroem.local: specifies the domain name in which domain controllers are being located

 So the full query means -> Locate all domain controllers in the klarstroem.local domain that provide LDAP services over TCP.

 When the query is recieved by the DNS server, the DNS service searches for SRV records that match this above **exact query name: - _ldap._tcp.dc._msdcs.klarstroem.local**

If the DNS server holds multiple DNS SRV records that match that exact query name, then multiple domain controllers will be returned. In my lab I have two domain controllers providing this exact service, and therefore it should return both:
- DC01.klarstroem.local
- DC02.klarstroem.local

I wasn't satisfied, I wanted to know exactly where the DNS server stores these exact SRV records, and therefore I follwed this path in the DNS Manager on my domain controller:
- Forward Lookup Zones -> _msdcs.klarstroem.local -> dc -> _tcp
Inside this location we see two LDAP SRV records because I have deployed two domain controllers:
![SRV records ldap](screenshots/srvrecords.png)

Each records holds:
- Service: LDAP
- Protocol: TCP
- The dc.klarstroem.local string
If we combine all of these we get exactly the above mentioned query: **_ldap._tcp.dc._msdcs.klarstroem.local**

This is why when the query request and the SRV record match then the SRV records returns the hostnames seen at the bottom of the SRV record:
![Domain controllers proving ldap](screenshots/ldaphosts.png)


## Verification

## Results

## Lessons Learned

## Next steps
