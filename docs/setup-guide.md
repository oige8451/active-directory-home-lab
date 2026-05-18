# Lab Setup Guide

A full walkthrough of how I built this Active Directory 
home lab environment from scratch using VirtualBox and 
Windows Server 2022.

---

## Tools and software used

| Tool | Purpose |
|---|---|
| VirtualBox 7.2.8 | Hypervisor to run virtual machines |
| Windows Server 2022 Standard Evaluation | Domain Controller OS |
| Windows 10 Pro | Client machine OS |

---

## Step 1 — Install VirtualBox

Downloaded VirtualBox 7.2.8 from virtualbox.org and 
installed it on the host Windows machine. VirtualBox 
is a free and open source hypervisor that allows 
multiple virtual machines to run simultaneously on 
a single physical computer.

---

## Step 2 — Create the Domain Controller VM (DC01)

Created a new virtual machine in VirtualBox with the 
following specifications:

| Setting | Value |
|---|---|
| Name | DC01 |
| OS | Windows Server 2022 64-bit |
| RAM | 4096 MB |
| CPU | 2 cores |
| Storage | 50 GB virtual hard disk |
| Network | Internal Network — AD-Lab |

Attached the Windows Server 2022 ISO and installed 
the operating system selecting the Desktop Experience 
edition to get a full GUI interface.

After installation I configured a static IP address:

| Setting | Value |
|---|---|
| IP Address | 192.168.1.1 |
| Subnet Mask | 255.255.255.0 |
| DNS Server | 127.0.0.1 |

Renamed the computer to DC01 and restarted.

---

## Step 3 — Install Active Directory Domain Services

Opened Server Manager and used the Add Roles and 
Features wizard to install the Active Directory 
Domain Services role along with DNS Server.

After installation clicked the yellow flag notification 
in Server Manager and selected Promote this server to 
a domain controller.

Configured the following:
- Deployment operation: Add a new forest
- Root domain name: corp.local
- Forest functional level: Windows Server 2016
- Domain functional level: Windows Server 2016
- DSRM password configured
- DNS Server role installed automatically

The server restarted automatically after promotion 
and logged back in as CORP\Administrator.

---

## Step 4 — Build the OU and user structure

Opened Active Directory Users and Computers from 
Server Manager → Tools and created the following 
Organizational Unit structure:
corp.local
├── IT
│   ├── jsmith (John Smith)
│   ├── alopez (Anna Lopez)
│   ├── IT-Admins (Security Group)
│   └── VPN-Users (Security Group)
├── HR
│   ├── smiller (Sarah Miller)
│   └── jtaylor (James Taylor)
├── Finance
│   ├── mchen (Mike Chen)
│   └── lbrown (Lisa Brown)
├── Helpdesk
└── Disabled-Users

All user accounts were created with:
- Temporary password: Welcome@123
- User must change password at next logon: enabled

Security groups created:
- IT-Admins: contains jsmith and alopez
- VPN-Users: contains IT department staff

---

## Step 5 — Configure Group Policy

Opened Group Policy Management from Server Manager 
→ Tools and created two GPOs:

### Password-Policy (linked to corp.local)

| Setting | Value |
|---|---|
| Minimum password length | 12 characters |
| Maximum password age | 90 days |
| Minimum password age | 1 day |
| Password complexity | Enabled |
| Password history | 10 passwords remembered |
| Account lockout threshold | 5 invalid attempts |
| Account lockout duration | 30 minutes |
| Reset lockout counter after | 30 minutes |

### HR-Desktop-Restrictions (linked to HR OU)

Applied user configuration restrictions to the HR 
OU to limit access to specific system settings for 
HR department users.

---

## Step 6 — Create the Client VM (CLIENT01)

Created a second virtual machine with the following 
specifications:

| Setting | Value |
|---|---|
| Name | CLIENT01 |
| OS | Windows 10 Pro 64-bit |
| RAM | 2048 MB |
| CPU | 2 cores |
| Storage | 40 GB virtual hard disk |
| Network | Internal Network — AD-Lab |

Configured a static IP address:

| Setting | Value |
|---|---|
| IP Address | 192.168.1.10 |
| Subnet Mask | 255.255.255.0 |
| DNS Server | 192.168.1.1 |

DNS points to DC01 at 192.168.1.1 — this is critical 
for domain join to work since Active Directory relies 
on DNS to locate the domain controller.

---

## Step 7 — Join CLIENT01 to the domain

Verified connectivity between CLIENT01 and DC01 by 
running a ping test:

gpupdate /force

Verify applied policies:

gpresult /r

Output confirmed the HR-Desktop-Restrictions GPO 
was listed under Applied Group Policy Objects for 
the HR user. Tested the restriction and confirmed 
it was enforcing as expected.

---

## Key concepts demonstrated

**Active Directory Domain Services (AD DS)**
The core Windows Server role that provides 
authentication and authorization for all users 
and computers in the domain.

**DNS**
Active Directory depends entirely on DNS to 
function. The Domain Controller runs DNS and 
CLIENT01 points to it for name resolution. 
Without correct DNS configuration domain joins 
and logins will fail.

**Organizational Units (OUs)**
OUs are containers within Active Directory used 
to organize users, computers, and groups. They 
also serve as the target for Group Policy 
application — different OUs can have different 
policies applied to them.

**Group Policy Objects (GPOs)**
GPOs enforce settings across users and computers 
in the domain. The Password-Policy GPO enforces 
password complexity and lockout rules for all 
domain users. The HR-Desktop-Restrictions GPO 
applies additional restrictions to HR department 
users only.

**Security Groups**
Security groups control access to resources. 
IT-Admins contains IT staff with elevated 
permissions. VPN-Users controls who has access 
to connect via VPN.

**Domain Join**
Joining a computer to the domain means it is 
now managed by Active Directory. Users can log 
in with their domain credentials and receive 
GPO settings automatically.
