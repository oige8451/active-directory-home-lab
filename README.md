# Active Directory Home Lab

A self-built Windows Server 2022 Active Directory environment 
deployed in VirtualBox, designed to simulate real-world Tier 1 
IT support workflows.

## Environment

| Component | Details |
|---|---|
| Hypervisor | VirtualBox 7.2.8 on Windows |
| Domain Controller | Windows Server 2022 — DC01 (192.168.1.1) |
| Client Machine | Windows 10 Pro — CLIENT01 (192.168.1.10) |
| Domain | corp.local |

## What I built

- Deployed Active Directory Domain Services and DNS on 
  Windows Server 2022
- Structured the domain with 4 OUs: IT, HR, Finance, Helpdesk
- Created 10+ user accounts and security groups 
  (IT-Admins, VPN-Users)
- Configured Group Policy: 12-character password minimum, 
  90-day expiry, 5-attempt lockout, desktop restrictions 
  for HR OU
- Joined a Windows 10 client to the corp.local domain
- Logged in as domain users and validated GPO enforcement
- Practiced core Tier 1 tasks: password resets, account 
  lockouts, user creation, group membership management

## Skills demonstrated

- Active Directory administration
- DNS configuration
- Group Policy creation and enforcement
- User and group lifecycle management
- Domain join and client configuration
- Network troubleshooting (static IP, internal networking)
- IT documentation and SOP writing

## Screenshots

### VirtualBox — both VMs created
![VMs](screenshots/01-virtualbox-vms.png)

### Windows Server desktop with Server Manager
![Server Desktop](screenshots/04-server-desktop.png)

### Static IP configured on DC01
![Static IP DC01](screenshots/05-dc01-static-ip.png)

### Active Directory Users and Computers — domain tree
![ADUC](screenshots/06-aduc-domain-tree.png)

### DNS Manager showing corp.local
![DNS](screenshots/07-dns-manager.png)

### OU structure across 4 departments
![OUs](screenshots/08-ou-structure.png)

### IT OU showing user accounts
![IT Users](screenshots/09-it-ou-users.png)

### IT-Admins security group members
![Groups](screenshots/10-it-admins-group.png)

### User management right-click menu
![User Management](screenshots/11-user-management.png)

### Password Policy GPO configured
![GPO Password](screenshots/12-gpo-password-policy.png)

### Account Lockout Policy configured
![GPO Lockout](screenshots/13-gpo-account-lockout.png)

### HR Desktop Restrictions GPO
![GPO HR](screenshots/14-gpo-hr-restrictions.png)

### Both GPOs linked in Group Policy Management
![GPO Linked](screenshots/15-gpo-linked.png)

### CLIENT01 static IP and DNS settings
![Static IP Client](screenshots/16-client01-static-ip.png)

### Ping test showing 0% loss
![Ping](screenshots/17-ping-success.png)

### Welcome to corp.local domain confirmation
![Domain Join](screenshots/18-domain-join-success.png)

### CLIENT01 login screen showing CORP domain
![Login Screen](screenshots/19-domain-login-screen.png)

### Logged in as domain user jsmith
![Domain User](screenshots/20-domain-user-login.png)

### GPO enforcement verified with gpresult
![GPResult](screenshots/21-gpresult-output.png)

### HR restriction enforced on CLIENT01
![HR Restriction](screenshots/22-gpo-restriction-enforced.png)


## Simulated Ticket Log

See [docs/ticket-log.md](docs/ticket-log.md)

## Setup Guide

See [docs/setup-guide.md](docs/setup-guide.md)
