# Active Directory Security & Endpoint Hardening Lab

# Introduction

This LAB showcases the design and enhancement of a Windows Active Directory (AD) environment, which forms the foundation of enterprise identity and access management within the Security Operations Environment. The objective of this project is to illustrate how AD security controls, baseline audit policies, and endpoint telemetry can be integrated to improve visibility, ensure compliance, and enhance threat detection across domain-joined systems.

# Architecture Overview
![architectu-image](https://github.com/user-attachments/assets/fecace20-3e55-498f-baa5-b26df05642bd)

# **Technologies:**

- **Active Directory**
- **Domain Controller**
- **Windows 10 Pro Client**
- **Sysmon (System Monitor)**
- **Splunk Universal Forwarder**
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
9. [Sysmon Installation](#sysmon-installation)
10. [Universal Splunk Forwarder](#universal-splunk-forwarder)
11. [Splunk (SIEM) Log Query Overview](#splunk-siem-log-query-overview)

---

# Active Directory Deployment

*Ref : Active Directory Installation*

![ADDC-O2](https://github.com/user-attachments/assets/47a94232-3985-4cab-a643-5e3b26a22e36)

![addc-03](https://github.com/user-attachments/assets/2a6b56a6-ebe0-4cbb-be34-4601df139de5)

![ADDC-O1](https://github.com/user-attachments/assets/74019068-e734-418e-87d6-42847b4fb0d6)

*Ref : Static IP Assigned for Server*

![static-ip1](https://github.com/user-attachments/assets/5a1edbc3-e605-4eec-8675-338f28ee7c37)

*Ref : IP Configuration and reachability test(Pfsense as our default DNS)*

![config-test](https://github.com/user-attachments/assets/9b578642-bb97-4e65-be17-81d5ff2bec14)

*Ref : Provisioning Users into their respective Organizational Units (OUs)*

![image-users](https://github.com/user-attachments/assets/c82f1cf1-e449-413f-8ef9-df377c9f2683)

---

# Deploying Microsoft baseline Audit GPO

By default, Windows systems only log a limited set of events, resulting in significant visibility gaps. For example, many environments are not following the recommendations, and default auditing may miss critical actions, such as privilege escalation, Kerberos abuse, or lateral movement.

We will review the recommended audit policies by operating system and categorise them accordingly. To proceed, access our Active Directory Domain Controller (ADDC) and open the Group Policy Management console. Under the forest containing our domain name, navigate to the domain section. From there, create a new policy. Right-click on the new policy and select "Edit." Name the new policy "Audit Policy - Endpoint."

Next, navigate to the Policies section, select Windows Settings, then Security Settings, and finally choose Advanced Audit Policy Configuration. This is where we will configure all our baseline settings.

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

In addition to implementing all the baseline recommendations, we must also enable process tracking, as process tracking is essential for making event ID 4688 as effective as it should be.

For example, let me go ahead and search up Event ID 4688, so by default, if we enable 4688, it will log the process name, but take a look at the process command line, it's empty, so without enabling process tracking, you essentially miss out on a lot of information. That is why we must allow process tracking.

By Default

![default-tracking](https://github.com/user-attachments/assets/f9f42e4b-77f8-427e-b0f0-5ca34f29d8f1)

After Process Logging

![after-tracking](https://github.com/user-attachments/assets/ec393f62-381a-4234-bbff-fa316a5c5ee0)

Now that we have everything in order, we will also enable PowerShell logging. To do that, I'll scroll down and expand the Windows Components section. What we want to look for is Windows PowerShell, which is right here. We have Turn Module Logging Script Block Logging. This one is the Event ID 4104. I'll click on enabled. I'll log the start and stop event and apply OK.

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

*Ref : Windows 10pro Setup & IP Assigning*


![static-ip2](https://github.com/user-attachments/assets/aaf19906-73b2-4658-81e0-9a0422f833f8)

Joining domain

*Ref : Successful Joining ADDC domain*

![domain-joined](https://github.com/user-attachments/assets/f0b1c25d-ccbe-4370-b963-56e476a9bd90)

---

# Sysmon Installation

Although we have updated our machines with the baseline recommendations provided by Microsoft, I'll still install and configure Sysmon to provide additional telemetry. In many environments, a system or even an EDR wouldn't be available. This is why it is essential to know how to enable proper logging on our machines to provide us with the telemetry that can help us during an investigation. Default settings are not enough.

![sysmon-install](https://github.com/user-attachments/assets/37f4790f-f3b2-4f76-bc3e-1fd5babb044c)

---

# Universal Splunk Forwarder

### Setting Up Universal Splunk Forwarder:

Installed and configured Splunk Forwarder on ADDC01 and target-PC (Windows 10) to send data to the Splunk server as a receiving indexer.

![splunk-forwarder](https://github.com/user-attachments/assets/be078ca2-1f8f-46c5-a8d6-638b00ecac6b)


### Configuring Inputs for Splunk Forwarder:

Created an inputs.conf file in C:\Program Files\SplunkUniversalForwarder\etc\system\local on ADDC01 and target-PC (Windows 10), Configuring settings.

![inputs-conf](https://github.com/user-attachments/assets/c33deaae-606d-4b5f-bc27-5a7c59c6846b)

### Configuring Splunk Forwarder Port:

To enable seamless log forwarding from Windows endpoints, Active Directory, and Sysmon, a Splunk receiving port will be configured.

![forwarder-port](https://github.com/user-attachments/assets/c8d65b27-c661-4af1-a22e-3cc9d65bc1e0)

![forwarder-port2](https://github.com/user-attachments/assets/cd407f3f-0a4c-4330-b82e-21813a7cf317)

### Restarting Splunk Forwarder Service:

Restarting the Splunk Forwarder under Services on ADDC01 AND Target-PC (Windows 10) as a local system account that way it can have all of the permission to read required logs by default the splunk forwarder account can not read Sysmon logs.

![services-restart](https://github.com/user-attachments/assets/4b4b0039-e497-42b7-8a5f-c85b7babac9f)

![services-restart2](https://github.com/user-attachments/assets/b2c8af47-2058-42dc-899d-385ef3b98fea)

#

## Splunk (SIEM) Log Query Overview

I am super excited everything looks good! As we can see, our logs are ready to be queried in our SIEM platform.

![splunk-log1](https://github.com/user-attachments/assets/70910d30-acb5-46fb-a660-933158445f63)
![splunk-log2](https://github.com/user-attachments/assets/fc393eca-ab83-4b63-a5d8-04bf5c83495b)

---

# Conclusion

I am excited about the progress made and the skills learn throughout the process. Let’s recap our achievements. We began with the deployment of Active Directory and provisioning users to different organisational units, followed by the implementation of the Microsoft baseline audit GPO. Next, we enabled the forced audit policy in local policies and joined our Windows 10 client to the ADDC.

We then configured Sysmon and the Splunk Universal Forwarder on both the ADDC and the Windows machine, including the port configurations for seamless log forwarding.

I am thrilled to see that everything is functioning correctly, and our logs are now ready to be queried in Splunk. This lab effectively demonstrates how the various components work together to build a strong defensive security posture, providing a solid foundation for cybersecurity best practices.

































