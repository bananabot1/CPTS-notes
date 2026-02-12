**Overview:**
- Used to get information about domain user and computer attributes, group membership, Group Policy Objects, permissions, ACLs, trusts, and more. 
- These tools will not work without valid domain user credentials at any permission level. 
---
## User Enumeration

```
rpcclient $> enumdomusers
```

Discovers user accounts and their associated relative identifiers (`RID`).

```
sudo nxc smb 172.16.5.5 -u <user> -p <password> --users
```

Authenticates over SMB with valid credentials and enumerates users in the target Windows domain.

---
## Share Enumeration

```
sudo nxc smb 172.16.5.5 -u <user> -p <password> <flag>
```

Enumerates the target over SMB using NetExec. `--groups` groups, `--loggedon-users` active sessions, `--shares` SMB shares.

```
smbmap -u <user> -p <password> -d <domain> -H 172.16.5.5
```

Enumerates shares and permissions on the target within the context of the valid credentials used. `-H` specifies the target host.

---
## Domain Admin Enumeration

```
python3 windapsearch.py --dc-ip 172.16.5.5 -u <domain>\<user> -p <password> --da
```

Enumerates the domain admins group (`--da`) using valid credentials on a target Windows domain.

---
## Remote Execution

```
psexec.py <domain>/<user>:<password>@172.16.5.125
```

Impacket tool used to connect to the CLI of a Windows target via the `ADMIN$` share with valid credentials.