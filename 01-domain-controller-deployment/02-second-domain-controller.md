# Deploying a second domain controller

## Overview
This lab documents the deployment of a second domain controller to our AD environment. The goal is to provide redundancy, enable replication between DC'S, and improve the resiliance of authentication and DNS services.

I the previous lab [domain controller deployment](https://github.com/RebinW/active-directory-domain-services/blob/main/01-domain-controller-deployment/01-domain-controller-deployment.md) I went through each step on how to install AD DS and how to promote the server to be a domain controller. In this module I wont document the same step, but instead focus on what to be aware of when deploying a second domain controller to an existing domain.

### Objectives
1. Understanding the importance of multiple domain controllers.
2. Configure a second domain controller in an existing domain.
3. Verify Active Directory replication.
4. Verify DNS replication between domain controllers.

### Environment
**Domain:** KlarStroem.local
**Servers:**
- DC01 - Primary domain controller
- DC02 - Additional domain controller
Network: 192.168.56.0/24
Technologies:
- VirtualBox
- Windows Server 2019
- Active Directory Domain Services
- DNS server
