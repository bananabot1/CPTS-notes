**Overview:**
- Deep-dive enumeration after obtaining domain credentials. Covers domain structure, trusts, group membership, SPNs, shares, and file discovery.
- The ActiveDirectory PowerShell module provides native Windows cmdlets for AD enumeration without third-party tooling. Useful when AV/EDR blocks external tools.
- PowerView provides more granular enumeration than the AD module and surfaces subtle misconfigurations in ACLs, trusts, and group relationships that BloodHound may not prioritize.
- Overly permissive shares frequently contain credentials, SSH keys, and configuration files. Always enumerate accessible shares and search for sensitive file content.
---
## ActiveDirectory Module

```powershell
Import-Module ActiveDirectory
Get-Module
```

Load the ActiveDirectory module and verify it is available. `Get-Module` lists all loaded modules with version and available commands.

```powershell
Get-ADDomain
```

Retrieve domain information including domain SID, functional level, PDC emulator, and infrastructure master.

```powershell
Get-ADTrust -Filter *
```

Enumerate all trust relationships for the current domain. `-Filter *` returns all trusts. Review direction (inbound/outbound/bidirectional) and transitivity for potential cross-domain attack paths.

```powershell
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
```

Enumerate users with a ServicePrincipalName set. These accounts are candidates for Kerberoasting.

```powershell
Get-ADGroup -Filter * | select name
```

List all groups in the domain by name.

```powershell
Get-ADGroup -Identity "<group-name>"
```

Retrieve detailed attributes for a specific group.

```powershell
Get-ADGroupMember -Identity "<group-name>"
```

List all members of a specific group.

---
## PowerView

```powershell
Import-Module .\PowerView.ps1
```

Load PowerView into the current PowerShell session. Will be blocked by Defender in default configurations so check security controls first.

```powershell
Get-DomainGroupMember -Identity "<group-name>" -Recurse
```

Enumerate all members of a group recursively. `-Recurse` resolves nested group membership, revealing indirect members that flat enumeration misses.

```powershell
Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
```

Enumerate users with SPNs set, returning only account name and SPN value. Feed output directly into Kerberoasting.

```powershell
Get-DomainTrust
```

Enumerate domain trusts for the current domain. Use `Get-ForestTrust` for forest-level trusts. Review for bidirectional or transitive trusts that expose lateral movement paths across domains.

---
## PowerView Commands

|Command|Description|
|---|---|
|`Export-PowerViewCSV`|Appends results to a CSV file.|
|`ConvertTo-SID`|Converts a user or group name to its SID.|
|`Get-DomainSPNTicket`|Requests the Kerberos ticket for a specified SPN.|
|`Get-Domain`|Returns the AD object for the current or specified domain.|
|`Get-DomainController`|Returns domain controllers for the target domain.|
|`Get-DomainUser`|Returns all users or specific user objects in AD.|
|`Get-DomainComputer`|Returns all computers or specific computer objects in AD.|
|`Get-DomainGroup`|Returns all groups or specific group objects in AD.|
|`Get-DomainOU`|Searches for all or specific OU objects in AD.|
|`Find-InterestingDomainAcl`|Finds object ACLs with modification rights set to non-built-in objects.|
|`Get-DomainGroupMember`|Returns the members of a specific domain group.|
|`Get-DomainFileServer`|Returns servers likely functioning as file servers.|
|`Get-DomainDFSShare`|Returns all distributed file systems for the domain.|
|`Get-DomainGPO`|Returns all GPOs or specific GPO objects in AD.|
|`Get-DomainPolicy`|Returns the default domain policy or DC policy.|
|`Get-NetLocalGroup`|Enumerates local groups on a local or remote machine.|
|`Get-NetLocalGroupMember`|Enumerates members of a specific local group.|
|`Get-NetShare`|Returns open shares on a local or remote machine.|
|`Get-NetSession`|Returns session information for a machine.|
|`Test-AdminAccess`|Tests if the current user has administrative access to a machine.|
|`Find-DomainUserLocation`|Finds machines where specific users are logged in.|
|`Find-DomainShare`|Finds reachable shares on domain machines.|
|`Find-InterestingDomainShareFile`|Searches for files matching criteria on readable shares.|
|`Find-LocalAdminAccess`|Finds machines where the current user has local admin access.|
|`Get-DomainTrust`|Returns domain trusts for the current or specified domain.|
|`Get-ForestTrust`|Returns all forest trusts for the current or specified forest.|
|`Get-DomainForeignUser`|Enumerates users in groups outside of the user's domain.|
|`Get-DomainForeignGroupMember`|Enumerates groups with users outside of the group's domain.|
|`Get-DomainTrustMapping`|Enumerates all trusts for the current domain and any others seen.|

---
## SharpView

```
.\SharpView.exe Get-DomainUser -Identity <user>
```

.NET port of PowerView. Use as a drop-in replacement when PowerShell execution is restricted or PowerView is blocked. Accepts the same arguments as PowerView cmdlets.

---
## Share Enumeration and File Discovery

```
.\Snaffler.exe -d <domain> -s -v data
```

Scan all shares accessible to the current user and surface files containing credentials, keys, or sensitive data. `-d` specifies the domain, `-s` enables share discovery, `-v data` sets verbosity to show file content hits.