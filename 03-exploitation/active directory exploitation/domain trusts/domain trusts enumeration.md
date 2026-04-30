**Overview:**
- Trusts link authentication systems between domains and are a common source of unintended attack paths. A compromised user in one domain may be able to access resources in a trusted domain, sometimes transitively across an entire forest.
- Transitive trusts extend automatically: if A trusts B and B trusts C transitively, A implicitly trusts C. Non-transitive trusts are scoped only to the directly linked domains.
- External trusts use SID filtering to block authentication requests from SIDs outside the trusted domain, which limits some cross-domain attacks. Forest and parent-child trusts do not filter by default, making them higher risk.
- Trust direction determines access flow: in a one-way trust, only users in the trusted domain can access the trusting domain. Bidirectional trusts allow access in both directions and double the attack surface.
- BloodHound can visualize trust relationships across the environment using the `Map Domain Trusts` pre-built query.

|Trust Type|Transitive|Direction|Notes|
|---|---|---|---|
|Parent-child|Yes|Bidirectional|Default within a forest|
|Cross-link|Yes|One or two-way|Speeds up auth between child domains|
|Tree-root|Yes|Bidirectional|Created when adding a new tree root|
|Forest|Yes|One or two-way|Between forest root domains|
|External|No|One or two-way|SID filtering applied|
|ESAE|No|One-way|Bastion forest for AD management|

---
## Trust Enumeration

```
Import-Module activedirectory
Get-ADTrust -Filter *
```

Enumerates all domain trusts using the built-in RSAT AD module. Useful when PowerView is unavailable.

```
Get-DomainTrust
```

Enumerates domain trusts using PowerView, including trust type and direction.

```
Get-DomainTrustMapping
```

Maps all reachable domain trusts recursively using PowerView, including parent-child, external, and forest trust types.

>**Netdom:**

```
netdom query /domain:<domain> trust
netdom query /domain:<domain> dc
netdom query /domain:<domain> workstation
```

Queries domain trusts, domain controllers, and workstations/servers for a given domain using the built-in `netdom` utility.

---
## User Enumeration Across Trusts

```
Get-DomainUser -Domain <trusted-domain> | select SamAccountName
```

Enumerates user accounts in a trusted domain using PowerView. Useful for identifying targets after establishing which trust paths are available.