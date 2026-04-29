**Overview:**
**Overview:**
Some of the data we gather in this phase may provide more information for reporting, not just directly lead to attack paths. Depending on the assessment type, our client may be interested in all possible findings, so even issues like the ability to run BloodHound freely or certain user account attributes may be worth including in our report as either medium-risk findings or a separate appendix section. Not every issue we uncover has to be geared towards forwarding our attacks. Some of the results may be informational in nature but useful to the customer to help improve their security posture.

At this point, we are interested in other misconfigurations and permission issues that could lead to lateral and vertical movement. We are also interested in getting a bigger picture of how the domain is set up, i.e., do any trusts exist with other domains both inside and outside the current forest? We're also interested in pillaging file shares that our user has access to, as these often contain sensitive data such as credentials that can be used to further our access.
The ActiveDirectory PowerShell module is a group of PowerShell cmdlets for administering an Active Directory environment from the command line. It consists of 147 different cmdlets at the time of writing. We can't cover them all here, but we will look at a few that are particularly useful for enumerating AD environments.
[PowerView](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon) is a tool written in PowerShell to help us gain situational awareness within an AD environment. Much like BloodHound, it provides a way to identify where users are logged in on a network, enumerate domain information such as users, computers, groups, ACLS, trusts, hunt for file shares and passwords, perform Kerberoasting, and more. It is a highly versatile tool that can provide us with great insight into the security posture of our client's domain. It requires more manual work to determine misconfigurations and relationships within the domain than BloodHound but, when used right, can help us to identify subtle misconfigurations.
Shares allow users on a domain to quickly access information relevant to their daily roles and share content with their organization. When set up correctly, domain shares will require a user to be domain joined and required to authenticate when accessing the system. Permissions will also be in place to ensure users can only access and see what is necessary for their daily role. Overly permissive shares can potentially cause accidental disclosure of sensitive information, especially those containing medical, legal, personnel, HR, data, etc. In an attack, gaining control over a standard domain user who can access shares such as the IT/infrastructure shares could lead to the disclosure of sensitive data such as configuration files or authentication files like SSH keys or passwords stored insecurely

---

---
## PowerShell Modules

```
Get-Module
```

Lists all available modules, their version and command options.

```
Import-Module ActiveDirectory
```

Loads the Active Directory PowerShell module.

---
## Domain Information

```
Get-ADDomain
```

Gathers Windows domain information. 

```
Get-ADTrust -Filter *
```

Enumerates any trust relationships in a target Windows domain. `-Filter *` returns all trusts. 

---
## User Enumeration

```
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
```

Enumerates user accounts and filters by `ServicePrincipalName`.

---
## Group Enumeration

```
Get-ADGroup -Filter * | select name
```

Enumerates groups in the target domain and filters by group name.

```
Get-ADGroup -Identity "Backup Operators"
```

Searches for a specific group.

```
Get-ADGroupMember -Identity "Backup Operators"
```

Discovers the members of a specific group.

---
## PowerView

```
Import-Module .\PowerView.ps1
```

Imports PowerView into the current PowerShell session.

> [!info]- PowerView Commands Reference
> 
> |Command|Description|
> |---|---|
> |`Export-PowerViewCSV`|Appends results to a CSV file|
> |`ConvertTo-SID`|Converts a User or Group name to its SID|
> |`Get-DomainSPNTicket`|Requests the Kerberos ticket for a specified SPN|
> |`Get-Domain`|Returns the AD object for the current or specified domain|
> |`Get-DomainController`|Returns domain controllers for the target domain|
> |`Get-DomainUser`|Returns all users or specific user objects in AD|
> |`Get-DomainComputer`|Returns all computers or specific computer objects in AD|
> |`Get-DomainGroup`|Returns all groups or specific group objects in AD|
> |`Get-DomainOU`|Searches for all or specific OU objects in AD|
> |`Find-InterestingDomainAcl`|Finds object ACLs with modification rights set to non-built-in objects|
> |`Get-DomainGroupMember`|Returns the members of a specific domain group|
> |`Get-DomainFileServer`|Returns servers likely functioning as file servers|
> |`Get-DomainDFSShare`|Returns all distributed file systems for the domain|
> |`Get-DomainGPO`|Returns all GPOs or specific GPO objects in AD|
> |`Get-DomainPolicy`|Returns the default domain policy or DC policy|
> |`Get-NetLocalGroup`|Enumerates local groups on a local or remote machine|
> |`Get-NetLocalGroupMember`|Enumerates members of a specific local group|
> |`Get-NetShare`|Returns open shares on a local or remote machine|
> |`Get-NetSession`|Returns session information for a machine|
> |`Test-AdminAccess`|Tests if current user has administrative access|
> |`Find-DomainUserLocation`|Finds machines where specific users are logged in|
> |`Find-DomainShare`|Finds reachable shares on domain machines|
> |`Find-InterestingDomainShareFile`|Searches for files matching criteria on readable shares|
> |`Find-LocalAdminAccess`|Finds machines where current user has local admin access|
> |`Get-DomainTrust`|Returns domain trusts for current or specified domain|
> |`Get-ForestTrust`|Returns all forest trusts for current or specified forest|
> |`Get-DomainForeignUser`|Enumerates users in groups outside of user's domain|
> |`Get-DomainForeignGroupMember`|Enumerates groups with users outside of group's domain|
> |`Get-DomainTrustMapping`|Enumerates all trusts for current domain and any others seen|
### Common PowerView Commands

```
Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```

Lists all members of a target group through recursion.

```
Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
```

Finds users on the target domain that have the Service Principal Name set. 


---
```
.\SharpView.exe Get-DomainUser -Identity forend
```
use SharpView to enumerate information about a specific user, such as the user `forend`, which we control.

## Snaffler

```
.\Snaffler.exe -d <domain> -s -v data
```

Finds various kinds of data in shares that the compromised account has access to.