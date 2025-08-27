# Active Directory Security & Endpoint Hardening Lab

# Introduction

This LAB showcases the design and enhancement of a Windows Active Directory (AD) environment, which forms the foundation of enterprise identity and access management within the Security Operations Environment. The objective of this project is to illustrate how AD security controls, baseline audit policies, and endpoint telemetry can be integrated to improve visibility, ensure compliance, and enhance threat detection across domain-joined systems.

# Overview

[ Domain Controller ]
   - Active Directory DS
   - DNS
   - Baseline Security Audit GPO

     │
        ▼

[ Windows 10 Client ]
   - Domain Joined
   - Receives GPOs
   - Runs Sysmon for telemetry
   - Installed Splunk Universal Forwarder




# **Technologies / Stacks:**

- **Active Directory**
  - Active Directory Domain Services (AD DS)
  - Active Directory Users and Computers (ADUC)
  - DNS (Domain Name System) for domain resolution
- **Domain Controller**
  - Windows Server providing Active Directory and authentication services
- **Windows 10 Pro Client**
  - Domain-joined endpoint for testing Group Policy Objects (GPOs) and telemetry
- **Sysmon (System Monitor)**
  - Advanced endpoint telemetry for monitoring process, network, and registry events
- **Splunk Universal Forwarder**
  - Log forwarding agent used to send Active Directory, Sysmon, and Windows logs to Splunk SIEM (Security Information and Event Management)
#

# Why Active Directory Matters

Active Directory serves as the central hub for managing users, computers, and security policies in most enterprise networks. By securing Active Directory and extending its visibility into endpoints, defenders gain improved control over network security.
  
  - Centralized control over users, groups, and workstations
  - Enforced security policies via Group Policy Objects (GPOs)
  - Comprehensive audit logs for accountability and compliance
  - Extended visibility into endpoint behavior with Sysmon
  - Integration with SIEMs (e.g., Splunk) for real-time detection


# What This LAB Covers

This focuses on enhancing and strengthening the Windows components of the SOC Lab, specifically:

   - Active Directory Setup – Domain Controller, OUs, and client domain-join
   - Baseline Security Auditing – Microsoft-recommended audit policy via GPO
   - Sysmon Deployment – Advanced endpoint telemetry for process, network, and registry monitoring
   - Splunk Universal Forwarder – Secure log forwarding from endpoints to the SIEM


*Ref : Active Directory Installation*

![ADDC-O1](https://github.com/user-attachments/assets/74019068-e734-418e-87d6-42847b4fb0d6)

*Ref : Static IP Assigned for Server*

![static-ip1](https://github.com/user-attachments/assets/5a1edbc3-e605-4eec-8675-338f28ee7c37)

IP Configuration and reachability test(Pfsense as our default DNS)

![config-test](https://github.com/user-attachments/assets/9b578642-bb97-4e65-be17-81d5ff2bec14)

Provisioning Users into their respective Organizational Units (OUs)

![image-users](https://github.com/user-attachments/assets/c82f1cf1-e449-413f-8ef9-df377c9f2683)

Windows 10pro Setup & IP Assigning

*Ref*
![static-ip2](https://github.com/user-attachments/assets/aaf19906-73b2-4658-81e0-9a0422f833f8)

Joining domain

*Ref : Successful Joining ADDC domain*

![domain-joined](https://github.com/user-attachments/assets/f0b1c25d-ccbe-4370-b963-56e476a9bd90)



---





























