**Overview:**
- RDP (Remote Desktop Protocol) is a remote access/management protocol that gives us GUI access to a target host.
- WinRM (PowerShell Remoting):  remote access protocol to run commands or enter an interactive command-line session on a remote host using PowerShell.
- MSSQL Server: Accounts with sysadmin privileges can log into the instance remotely and execute queries against the database.
---
# Linux
## RDP

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