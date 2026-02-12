**Overview:**
- After gaining a foothold, its possible to enumerate the defensive state of the hosts, enumerate the domain further now that visibility is not as restricted.
- **These commands are performed from a Windows host.**
---
## Windows Defender

```
Get-MpComputerStatus
```

Checks the status of Windows Defender Anti-Virus.

---
## LAPS Enumeration

```
Find-AdmPwdExtendedRights
```

Checks rights on LAPS-enabled computers for groups with read access and users with All Extended Rights. (`LAPSToolkit`).

```
Get-LAPSComputers
```

Searches for LAPS-enabled computers, discovers password expiration dates, and can reveal randomized passwords. (`LAPSToolkit`).

---
## PowerShell Language Mode

```
$ExecutionContext.SessionState.LanguageMode
```

Discovers the PowerShell Language Mode being used on a Windows-based host.