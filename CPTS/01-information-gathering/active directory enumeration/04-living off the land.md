**Overview:**
- A Living Off the Land (LOTL) attack is a technique where adversaries use legitimate, native system tools and features that are already installed on target systems to carry out malicious activities.
- **These commands are performed from a Windows host.**
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

**Commands for kerberoasting:**
[[kerberoasting#PowerView]]

---
## Snaffler

```
.\Snaffler.exe -d <domain> -s -v data
```

Finds various kinds of data in shares that the compromised account has access to.