# Active Directory Security & Endpoint Hardening Lab

# Introduction

This LAB showcases the design and enhancement of a Windows Active Directory (AD) environment, which forms the foundation of enterprise identity and access management within the Security Operations Environment. The objective of this project is to illustrate how AD security controls, baseline audit policies, and endpoint telemetry can be integrated to improve visibility, ensure compliance, and enhance threat detection across domain-joined systems.

# Architecture Overview
![architectu-image](

# **Technologies:**

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
#

# Table Of Content

1. [Introduction](#introduction)
2. [Architecture Overview](#architecture-overview)
3. [Technologies](#technologies)
4. [Why Active Directory Matters](#why-active-directory-matters)
5. [Active Directory Deployment](#active-directory-deployment)
6. [Deploying Microsoft baseline Audit GPO](#deploying-microsoft-baseline-audit-gpo)
7. [Advanced Audit Policy Configuration Settings](#advanced-audit-policy-configuration-settings)
8. [Windows 10 Client](#windows-10-client)
9. [Sysmon Deployment](#sysmon-deployment)
10. [Splunk Forwarder](#splunk-forwarder)

---

# Active Directory Deployment

*Ref : Active Directory Installation*

![ADDC-O1](https://github.com/user-attachments/assets/74019068-e734-418e-87d6-42847b4fb0d6)

*Ref : Static IP Assigned for Server*

![static-ip1](https://github.com/user-attachments/assets/5a1edbc3-e605-4eec-8675-338f28ee7c37)

IP Configuration and reachability test(Pfsense as our default DNS)

![config-test](https://github.com/user-attachments/assets/9b578642-bb97-4e65-be17-81d5ff2bec14)

Provisioning Users into their respective Organizational Units (OUs)

![image-users](https://github.com/user-attachments/assets/c82f1cf1-e449-413f-8ef9-df377c9f2683)

---

# Deploying Microsoft baseline Audit GPO

By default, Windows systems only log a limited set of events, resulting in significant visibility gaps. For example, many environments are not following the recommendations, and default auditing may miss critical actions, such as privilege escalation, Kerberos abuse, or lateral movement.

We will review the recommended audit policies by operating system and categorise them accordingly. To proceed, access our Active Directory Domain Controller (ADDC) and open the Group Policy Management console. Under the forest containing our domain name, navigate to the domain section. From there, create a new policy. Right-click on the new policy and select "Edit." Name the new policy "Audit Policy - Endpoint."

Next, go to the policies section, choose Windows Settings, then Security Settings, and finally select Advanced Audit Policy Configuration. This is where we will configure all our baseline settings.

![gpo-recomm1](https://github.com/user-attachments/assets/82e195cc-c509-4ba7-8e2d-379c01765290)

![gpo-recom2](https://github.com/user-attachments/assets/b1071f1f-8202-425d-b32c-058a21069ab8)

Account Logon Tracking

![account-logon1](https://github.com/user-attachments/assets/21181873-af40-4901-b2cf-0b6287a2a25e)

![account-logon2](https://github.com/user-attachments/assets/6acd060c-9820-4baf-96f3-357bdc555602)

Account Management Tracking

![account-managem1](https://github.com/user-attachments/assets/e2c88296-bd25-48c8-93f7-f7708e5088aa)

![account-managem2](https://github.com/user-attachments/assets/74d12112-5269-49f2-a81b-0b796e7739aa)

Detailed Tracking

![details-tracking1](https://github.com/user-attachments/assets/5d164bd2-a189-44f2-acb0-20ce00ee717f)

![details-tracking2](https://github.com/user-attachments/assets/09135ccc-2816-4175-bd46-76e2e2a0637f)

Logon and Logoff Tracking

![logon-logof1](https://github.com/user-attachments/assets/cc6058da-d6fe-4805-94be-9783c517b8d2)

![logon-logof1](https://github.com/user-attachments/assets/212bb5d0-23e9-4597-a76c-64271af8f26b)

Policy Change Tracking

![policy-change1](https://github.com/user-attachments/assets/e9685810-45c2-4963-bb75-7c26d1a6f223)

![policy-change12](https://github.com/user-attachments/assets/6ba862a1-1684-4a6e-a20a-56b7d4411a05)

System Tracking

![system-track](https://github.com/user-attachments/assets/3dafb84b-7254-4187-8787-66d1b5c73a62)

![system-track2](https://github.com/user-attachments/assets/5fc6e494-89af-4fa1-9820-755a79f80745)
#

In addition to enabling all the baseline recommendations, we must also allow process tracking, because without process tracking, event ID 4688 is not as effective as it should be.

For example, let me go ahead and search up Event ID 4688, so by default, if we enable 4688, it will log the process name, but take a look at the process command line it's empty so without enabling process tracking you essentially miss out on a lot of information that is why we must allow process tracking.

By Default

![default-tracking](https://github.com/user-attachments/assets/f9f42e4b-77f8-427e-b0f0-5ca34f29d8f1)

After Process Logging

![after-tracking](https://github.com/user-attachments/assets/ec393f62-381a-4234-bbff-fa316a5c5ee0)

So now we have everything pretty much good to go and just remember that we should enable Powershell logging as well just in case and to do that I'll Scroll down and expand Windows Components what we want to look for is Windows Powershell which is right here and we have Turn Module Logging Script Block Logging this one is the Event ID 4104 I'll click on enabled. I'll log the start and stop event and apply OK.

![powershel-log](https://github.com/user-attachments/assets/b729b754-2422-4b89-b076-1b0fe4f23f6e)

# Advanced Audit Policy Configuration Settings

The final crucial step involves ensuring that the audit force audit policy is enabled when using advanced audit policy configuration settings. Since we utilized these advanced configurations, we manually set up the specific items we want to audit. This means we need to enable the audit force audit policy setting. 

To locate this setting, navigate to Local Policy and then select Security Options. Look for the "Audit Force" option. Once you find it, double-click on it to access the policy settings, and select "Enabled."

![audit-policy1](https://github.com/user-attachments/assets/985a3b7b-6ff6-412c-9429-ad1107915829)
![audit-policy](https://github.com/user-attachments/assets/a996610b-d46f-427a-b29e-067dbd52fb41)

#
We need to link our Baseline Security Auditing GPO to our OU. By enabling this, all our workstations/endpoints will automatically collect the update.

![gpo-OU](https://github.com/user-attachments/assets/867db7d3-f7db-4e44-917a-42ae4f1c1356)

---

# Windows 10 Client

Windows 10pro Setup & IP Assigning

*Ref*
![static-ip2](https://github.com/user-attachments/assets/aaf19906-73b2-4658-81e0-9a0422f833f8)

Joining domain

*Ref : Successful Joining ADDC domain*

![domain-joined](https://github.com/user-attachments/assets/f0b1c25d-ccbe-4370-b963-56e476a9bd90)

---

# Sysmon Deployment

Although we have updated our machines with the baseline recommendations provided by Microsoft, I'll still install and configure Sysmon to provide additional telemetry. In many environments, a system or even an EDR wouldn't be available. This is why it is essential to know how to enable proper logging on our machines to provide us with the telemetry that can help us during an investigation. Default settings are not enough.

![sysmon-install](https://github.com/user-attachments/assets/6f0965a3-5495-4c21-8695-051c12926157)

---

# Splunk Forwarder

Created an inputs.conf file in C:\Program Files\SplunkUniversalForwarder\etc\system\local on Windows Machine and ADDC01, Configuring settings.




























