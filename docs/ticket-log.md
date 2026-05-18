# Help Desk Ticket Log

Simulated help desk tickets practiced in my Active Directory 
home lab environment. Each ticket reflects real Tier 1 help 
desk scenarios resolved using Active Directory Users and 
Computers, Group Policy Management, and Windows 10 client 
troubleshooting.

---

## Ticket #001 — Account Lockout

**Date:** 2026-01-15
**Reported by:** Sarah Miller (HR Department)
**Priority:** P2 — High
**Status:** Resolved
**Resolved by:** Olamide Ige
**Time to resolve:** 4 minutes

**What the user reported:**
Sarah called the help desk saying she could not log into her 
computer. She had tried her password several times and kept 
getting an error saying her account was locked.

**What I did:**
I asked Sarah to confirm her username so I could locate her 
account. I opened Active Directory Users and Computers on DC01 
and navigated to the HR OU. I found her account, double-clicked 
it, and went to the Account tab. The Unlock account checkbox was 
ticked, confirming the lockout. I unchecked it to unlock the 
account and clicked OK.

I then asked Sarah if she had recently changed her password, 
because saved credentials on her phone or another device could 
be repeatedly trying to authenticate with the old password and 
triggering the lockout. She confirmed she had changed it the day 
before and had not updated it on her work phone. I advised her 
to update the saved password on all her devices to prevent the 
lockout from happening again.

I asked her to attempt login and she confirmed it was successful.

**Root cause:** Saved credentials on a secondary device 
attempting authentication with an outdated password, triggering 
the 5-attempt lockout policy.

**Prevention note:** Advised user to update saved passwords on 
all devices immediately after any password change.

---

## Ticket #002 — Password Reset Request

**Date:** 2026-01-16
**Reported by:** James Taylor (HR Department)
**Priority:** P2 — High
**Status:** Resolved
**Resolved by:** Olamide Ige
**Time to resolve:** 3 minutes

**What the user reported:**
James submitted a ticket saying he had forgotten his password 
after returning from a two week vacation and could not log in.

**What I did:**
Before making any changes I verified James's identity by 
confirming his full name, department, and employee ID. Once 
verified I navigated to his account in the HR OU in Active 
Directory Users and Computers. I right-clicked his account 
and selected Reset Password. I set a temporary password of 
Welcome@2024! and checked the box for User must change 
password at next logon so he would be prompted to create 
his own secure password immediately.

I communicated the temporary password to James securely over 
the phone rather than sending it by email, following security 
best practices. I stayed on the call while he logged in 
successfully and completed the password change.

**Root cause:** User forgot password after extended absence.

**Prevention note:** Reminded user of the 90-day password 
expiry policy and suggested using a password manager to 
avoid future lockouts.

---

## Ticket #003 — New Employee Account Setup

**Date:** 2026-01-20
**Reported by:** HR Department
**Priority:** P2 — High
**Status:** Resolved
**Resolved by:** Olamide Ige
**Time to resolve:** 12 minutes

**What the user reported:**
HR contacted the help desk to request an account be created 
for a new IT hire, John Smith, who was starting on Monday. 
They needed his account, email, and domain access ready 
before his first day.

**What I did:**
I opened Active Directory Users and Computers and navigated 
to the IT OU. I right-clicked and selected New → User. I 
filled in John's full name and set his username to jsmith 
following the company's firstname initial + lastname naming 
convention. I set a temporary password and checked User must 
change password at next logon.

After creating the account I double-clicked it and went to 
the Member Of tab. I added jsmith to the IT-Admins and 
VPN-Users security groups based on the access requirements 
provided by HR. I verified his group membership was correct 
before closing.

I then confirmed the account was working by logging into 
CLIENT01 using his credentials. Windows prompted him to 
change his password on first login as expected. I documented 
the completed setup and sent HR a confirmation email with 
his username and instructions for his first day.

**Root cause:** N/A — standard onboarding request.

**Notes:** Account created, group memberships confirmed, 
and first login tested successfully before start date.

---

## Ticket #004 — Cannot Connect to Domain

**Date:** 2026-01-22
**Reported by:** IT Department (self-identified during lab setup)
**Priority:** P1 — Critical
**Status:** Resolved
**Resolved by:** Olamide Ige
**Time to resolve:** 25 minutes

**What the user reported:**
CLIENT01 was unable to join the corp.local domain. The error 
message stated the domain could not be contacted.

**What I did:**
I started by checking the basics. I opened Command Prompt on 
CLIENT01 and ran ipconfig to check the network configuration. 
The DNS server was not pointing to DC01 at 192.168.1.1, which 
meant CLIENT01 could not resolve the corp.local domain name.

I opened Network adapter settings and updated the IPv4 
configuration to set the preferred DNS server to 192.168.1.1. 
I then ran a ping test to 192.168.1.1 to confirm CLIENT01 
could reach DC01. The ping returned 4 out of 4 packets with 
0% loss confirming connectivity.

I also identified that both VMs were on different virtual 
networks in VirtualBox. I shut down both VMs and changed 
their network adapters from NAT to Internal Network using 
the same network name AD-Lab. After rebooting both VMs and 
confirming the ping was successful I attempted the domain 
join again. CLIENT01 successfully joined corp.local and 
displayed the Welcome to the corp.local domain confirmation.

**Root cause:** Incorrect DNS configuration on CLIENT01 and 
VMs placed on separate virtual networks preventing 
communication.

**Prevention note:** Always verify DNS points to the Domain 
Controller and confirm VM network settings before attempting 
a domain join.

---

## Ticket #005 — GPO Not Applying on Client Machine

**Date:** 2026-01-23
**Reported by:** IT Department (self-identified during testing)
**Priority:** P3 — Medium
**Status:** Resolved
**Resolved by:** Olamide Ige
**Time to resolve:** 10 minutes

**What the user reported:**
After configuring the HR Desktop Restrictions GPO the policy 
did not appear to be enforcing on CLIENT01 for the HR user 
account.

**What I did:**
I logged into CLIENT01 as the HR user smiller and opened 
Command Prompt. I ran gpupdate /force to manually trigger 
a Group Policy refresh rather than waiting for the default 
90-minute automatic refresh interval.

After the refresh completed I ran gpresult /r to verify 
which policies were being applied to the current user and 
machine. The output confirmed the HR-Desktop-Restrictions 
GPO was now listed under Applied Group Policy Objects. I 
tested the restriction by attempting to open the restricted 
setting and confirmed it was blocked as expected.

**Root cause:** Group Policy had not yet refreshed after 
the initial GPO link. The default refresh interval is 
90 minutes for domain computers.

**Prevention note:** When testing new GPOs always run 
gpupdate /force on the client machine to apply policies 
immediately rather than waiting for the automatic refresh.

---

## Ticket #006 — Offboarding a Departed Employee

**Date:** 2026-01-25
**Reported by:** HR Department
**Priority:** P2 — High
**Status:** Resolved
**Resolved by:** Olamide Ige
**Time to resolve:** 6 minutes

**What the user reported:**
HR contacted the help desk to disable the account of a 
Finance employee, Lisa Brown, who had resigned effective 
immediately. They needed her access revoked right away.

**What I did:**
I immediately located lbrown's account in the Finance OU 
in Active Directory Users and Computers. I right-clicked 
her account and selected Disable Account to revoke her 
access instantly. I then double-clicked her account and 
went to the Member Of tab and removed her from all security 
groups to ensure she had no residual access to any systems 
or shared resources.

I created a new OU called Disabled-Users under corp.local 
to store deprovisioned accounts for the standard 90-day 
retention period before permanent deletion per company 
policy. I moved lbrown's account into this OU.

I confirmed with HR that the account was disabled and 
documented the time of deactivation, the groups she was 
removed from, and the date the account is scheduled for 
permanent deletion.

**Root cause:** N/A — standard offboarding request.

**Notes:** Account disabled immediately upon request. 
All group memberships removed. Account moved to 
Disabled-Users OU for 90-day retention.

---

## Ticket #007 — User Needs VPN Access

**Date:** 2026-01-27
**Reported by:** John Smith (IT Department)
**Priority:** P3 — Medium
**Status:** Resolved
**Resolved by:** Olamide Ige
**Time to resolve:** 5 minutes

**What the user reported:**
John Smith contacted the help desk saying he was working 
from home for the first time and could not connect to the 
company VPN. He had not been added to the VPN access group.

**What I did:**
I opened Active Directory Users and Computers and located 
jsmith in the IT OU. I double-clicked his account and went 
to the Member Of tab. I confirmed he was not currently a 
member of the VPN-Users security group. I clicked Add and 
typed VPN-Users, clicked Check Names to verify the group 
name resolved correctly, and clicked OK to add him.

I advised John to attempt the VPN connection again and 
confirmed with him that it was now working successfully. 
I documented the group membership change and the time it 
was made.

**Root cause:** User had not been added to the VPN-Users 
security group during initial onboarding.

**Prevention note:** Updated the onboarding checklist to 
include VPN-Users group membership for all IT department 
staff automatically.

---

## Ticket #008 — Wrong User in Wrong OU

**Date:** 2026-01-28
**Reported by:** IT Department (self-identified during audit)
**Priority:** P4 — Low
**Status:** Resolved
**Resolved by:** Olamide Ige
**Time to resolve:** 3 minutes

**What the user reported:**
During a routine audit of the Active Directory structure I 
noticed a user account had been created in the default 
Users container instead of the correct department OU. This 
meant the user was not receiving the correct Group Policy 
settings for their department.

**What I did:**
I located the misplaced account in the default Users 
container in Active Directory Users and Computers. I 
right-clicked the account and selected Move. I selected 
the correct department OU and clicked OK. The account 
was immediately moved and began receiving the correct 
GPO settings for that OU on the next Group Policy refresh.

I ran gpupdate /force on the affected machine to apply 
the correct policies immediately and verified with 
gpresult /r that the right GPOs were now applied.

**Root cause:** Account created in wrong location during 
initial setup.

**Prevention note:** Always double-check the target OU 
before creating a new user account. Consider using a 
standardized user creation checklist to prevent similar 
issues during onboarding.
