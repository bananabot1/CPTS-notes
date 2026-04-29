**Overview:**
Some of the data we gather in this phase may provide more information for reporting, not just directly lead to attack paths. Depending on the assessment type, our client may be interested in all possible findings, so even issues like the ability to run BloodHound freely or certain user account attributes may be worth including in our report as either medium-risk findings or a separate appendix section. Not every issue we uncover has to be geared towards forwarding our attacks. Some of the results may be informational in nature but useful to the customer to help improve their security posture.

At this point, we are interested in other misconfigurations and permission issues that could lead to lateral and vertical movement. We are also interested in getting a bigger picture of how the domain is set up, i.e., do any trusts exist with other domains both inside and outside the current forest? We're also interested in pillaging file shares that our user has access to, as these often contain sensitive data such as credentials that can be used to further our access.
The ActiveDirectory PowerShell module is a group of PowerShell cmdlets for administering an Active Directory environment from the command line. It consists of 147 different cmdlets at the time of writing. We can't cover them all here, but we will look at a few that are particularly useful for enumerating AD environments.

---
## ActiveDirectory PowerShell Module

```
Import-Module ActiveDirectory
PS C:\htb> Get-Module
```
