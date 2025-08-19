# Active Directory

# Introduction

We will explore the integration of Windows Server 2022 and Windows 10 Pro as part of this project. Our goal is to address various server challenges, including specific aspects such as power options and network settings. We will cover the elevation of Windows Server through its roles, the creation of an admin account, and the configuration of remote access. This will showcase the simplicity of account automation, the establishment of DHCP scopes, and the seamless integration of Windows clients into the domain. Additionally, we will demonstrate reachability through ping tests and lay the groundwork for a study on IT infrastructure. 
#

# Technologies/Stacks
- Active Directory(Active Directory Domain Services and Active Directory Users and Computers)
- DHCP
- DNS
- Remote Access
- Domain Controller
- NAT/RAS
#

# Key Functions of Active Directory

1. Authentication & Authorization
   - AD verifies a user’s identity when they log in (authentication)
   - It grants access to resources based on permissions (authorization)

2. Centralized Management
   - Administrators can create users, manage groups, reset passwords, and enforce security policies across the domain

3. Group Policy (GPO)
   - Enforces security baselines, audit policies, software installations, and system configurations automatically across all domain-joined devices

4. Scalability & Hierarchy
   - AD utilises a structured hierarchy (domains, organizational units, forests) to manage resources in both small and large organizations

#

*Ref : Active Directory Installation*

![ADDC-O1](https://github.com/user-attachments/assets/74019068-e734-418e-87d6-42847b4fb0d6)

*Ref : Static IP Assigned for Server*

![static-ip1](https://github.com/user-attachments/assets/5a1edbc3-e605-4eec-8675-338f28ee7c37)

IP Configuration and reachability test(Pfsense as our default DNS)

![config-test](https://github.com/user-attachments/assets/9b578642-bb97-4e65-be17-81d5ff2bec14)
