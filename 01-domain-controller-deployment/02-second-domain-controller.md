# Deploying a second domain controller

## Overview
After some consideration, I decided to deploy a second domain controller to our AD environment. The goal is to provide redundancy, enable replication between DC'S, and improve the resiliance of authentication and DNS services.

I the previous lab [domain controller deployment]() I went through each step on how to install AD DS and how to promote the server to be a domain controller. In this module I wont document the same step, but instead focus on what to be aware of when deploying a second domain controller to an existing domain.

### Objectives

1. 
