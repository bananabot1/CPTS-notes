- Security controls in place directly affect which tools and techniques are viable. Always enumerate defenses before running enumeration or exploitation tooling.
- Windows Defender blocks common tools like PowerView by default. Must be checked and bypassed before running any PowerShell-based AD enumeration.
- AppLocker commonly blocks `powershell.exe` and `cmd.exe` but organizations often overlook alternate PowerShell binary locations such as `%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe` and `PowerShell_ISE.exe`.
- LAPS randomizes and rotates local administrator passwords to prevent lateral movement. Identifying which users can read LAPS passwords and which hosts have LAPS disabled are both high-value findings.

> All commands run on the Windows target

---

## Windows Defender

powershell

```powershell
Get-MpComputerStatus
```

Check current Defender status. `RealTimeProtectionEnabled: True` confirms Defender is active and will block unsigned or known-malicious tooling.

---

## AppLocker

powershell

```powershell
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```

Retrieve effective AppLocker rules. Review which executables, scripts, and DLLs are blocked for Domain Users. Look for gaps such as unblocked alternate PowerShell paths or writable directories not covered by rules.

---

## PowerShell Language Mode

powershell

```powershell
$ExecutionContext.SessionState.LanguageMode
```

Check whether PowerShell is running in `FullLanguage` or `ConstrainedLanguage` mode. Constrained Language Mode blocks COM objects, restricts .NET types, and prevents many PowerShell-based attack tools from functioning. Indicates AppLocker or WDAC is enforced.

---

## LAPS Enumeration

powershell

```powershell
Find-LAPSDelegatedGroups
```

Identify groups explicitly delegated to read LAPS passwords across the domain.

powershell

```powershell
Find-AdmPwdExtendedRights
```

Identify users and groups with `All Extended Rights` on LAPS-enabled computers. Users with this right can read LAPS cleartext passwords and are often less monitored than members of delegated groups.

powershell

```powershell
Get-LAPSComputers
```

List computers with LAPS enabled, their password expiry dates, and cleartext passwords if the current user has sufficient rights. Computers without LAPS are candidates for local admin credential reuse attacks.

