**Overview:**
Once we gain a foothold in the domain, our goal shifts to advancing our position further by moving laterally or vertically to obtain access to other hosts, and eventually achieve domain compromise or some other goal, depending on the aim of the assessment. Typically, if we take over an account with local admin rights over a host, or set of hosts, we can perform a `Pass-the-Hash` attack to authenticate via the SMB protocol.

There are several other ways we can move around a Windows domain:

- `Remote Desktop Protocol` (`RDP`) - is a remote access/management protocol that gives us GUI access to a target host
- [PowerShell Remoting](https://docs.microsoft.com/en-us/powershell/scripting/learn/ps101/08-powershell-remoting?view=powershell-7.2) - also referred to as PSRemoting or Windows Remote Management (WinRM) access, is a remote access protocol that allows us to run commands or enter an interactive command-line session on a remote host using PowerShell
- `MSSQL Server` - an account with sysadmin privileges on an SQL Server instance can log into the instance remotely and execute queries against the database. This access can be used to run operating system commands in the context of the SQL Server service account through various methods

We can enumerate this access in various ways. The easiest, once again, is via BloodHound, as the following edges exist to show us what types of remote access privileges a given user has:

- [CanRDP](https://bloodhound.specterops.io/resources/edges/can-rdp)
- [CanPSRemote](https://bloodhound.specterops.io/resources/edges/can-ps-remote)
- [SQLAdmin](https://bloodhound.specterops.io/resources/edges/sql-admin)
Typically, if we have control of a local admin user on a given machine, we will be able to access it via RDP. Sometimes, we will obtain a foothold with a user that does not have local admin rights anywhere, but does have the rights to RDP into one or more machines. 
- Like RDP, we may find that either a specific user or an entire group has WinRM access to one or more hosts. This could also be low-privileged access that we could use to hunt for sensitive data or attempt to escalate privileges or may result in local admin access, which could potentially be leveraged to further our access
- More often than not, we will encounter SQL servers in the environments we face. It is common to find user and service accounts set up with sysadmin privileges on a given SQL server instance. We may obtain credentials for an account with this access via Kerberoasting (common) or others such as LLMNR/NBT-NS Response Spoofing or password spraying. Another way that you may find SQL server credentials is using the tool [Snaffler](https://github.com/SnaffCon/Snaffler) to find web.config or other types of configuration files that contain SQL server connection strings.
---
## RDP - Linux

```
Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Desktop Users"
```
use the [Get-NetLocalGroupMember](https://powersploit.readthedocs.io/en/latest/Recon/Get-NetLocalGroupMember/) function to begin enumerating members of the `Remote Desktop Users` group on a given host


```
xfreerdp /v:<target-ip> /u:<user> /p:<password> /drive:share,<local-path>
```

Connects to target via RDP and mounts a local directory as a shared drive. 

---
## WinRM

```
evil-winrm -i <target-ip> -u <user> -p <password>
```

Establishes a PowerShell session with a Windows target using WinRM. 

---
## MSSQL

```
mssqlclient.py <domain>/<user>@<target-ip>
```

Connects to an MSSQL server. Impacket tool. 

---
# Windows
## WinRM

```
$password = ConvertTo-SecureString "<password>" -AsPlainText -Force
```

Creates a SecureString variable for the password.

```
$cred = New-Object System.Management.Automation.PSCredential ("<domain>\<user>", $password)
```

Creates a PSCredential object with username and password.

```
Enter-PSSession -ComputerName <target> -Credential $cred
```

Establishes a PowerShell session with the target over WinRM.

---
## MSSQL

### PowerUpSQL

```
Import-Module .\PowerUpSQL.ps1
```

Imports the PowerUpSQL tool.

```
Get-SQLQuery -Verbose -Instance "<target-ip>,1433" -username "<domain>\<user>" -password "<password>" -query '<query>'
```

Connects to an SQL server and executes a query.