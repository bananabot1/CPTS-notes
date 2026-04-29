**Overview:**
- Enumeration techniques to do after having retrieved valid credentials.
We are interested in information about domain user and computer attributes, group membership, Group Policy Objects, permissions, ACLs, trusts, and more. We have various options available, but the most important thing to remember is that most of these tools will not work without valid domain user credentials at any permission level. So at a minimum, we will have to have acquired a user's cleartext password, NTLM password hash, or SYSTEM access on a domain-joined host.
While looking at users in rpcclient, you may notice a field called `rid:` beside each user. A [Relative Identifier (RID)](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/security-identifiers) is a unique identifier (represented in hexadecimal format) utilized by Windows to track and identify objects. To explain how this fits in, let's look at the examples below:

- The [SID](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/security-identifiers) for the INLANEFREIGHT.LOCAL domain is: `S-1-5-21-3842939050-3880317879-2865463114`.
- When an object is created within a domain, the number above (SID) will be combined with a RID to make a unique value used to represent the object.
- So the domain user `htb-student` with a RID:0x457 Hex 0x457 would = decimal `1111`, will have a full user SID of: `S-1-5-21-3842939050-3880317879-2865463114-1111`.
- This is unique to the `htb-student` object in the INLANEFREIGHT.LOCAL domain and you will never see this paired value tied to another object in this domain or any other.

However, there are accounts that you will notice that have the same RID regardless of what host you are on. Accounts like the built-in Administrator for a domain will have a RID administrator rid:0x1f4, which, when converted to a decimal value, equals `500`. The built-in Administrator account will always have the RID value `Hex 0x1f4`, or 500.

---
## User Enumeration

```
rpcclient $> enumdomusers
```

Discovers user accounts and their associated relative identifiers (`RID`).

```
sudo nxc smb 172.16.5.5 -u <user> -p <password> --users
```

Authenticates over SMB with valid credentials and enumerates users in the target Windows domain. We can also obtain a complete listing of domain groups --groups.
The output also shows the built-in groups on the Domain Controller, such as `Backup Operators`. We can begin to note down groups of interest. Take note of key groups like `Administrators`, `Domain Admins`, `Executives`, any groups that may contain privileged IT admins, etc. These groups will likely contain users with elevated privileges worth targeting during our assessment.
```
--loggedon-users
```
check out what appears to be a file server to see what users are logged in currently.

---
## Share Enumeration

```
sudo nxc smb 172.16.5.5 -u <user> -p <password> <flag>
```

Enumerates the target over SMB using NetExec. `--groups` groups, `--loggedon-users` active sessions, `--shares` SMB shares.
```
sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 -M spider_plus --share 'Department Shares'
```
The module `spider_plus` will dig through each readable share on the host and list all readable files

```
smbmap -u <user> -p <password> -d <domain> -H 172.16.5.5
```

Enumerates shares and permissions on the target within the context of the valid credentials used. `-H` specifies the target host.
```
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R 'Department Shares' --dir-only
```
Recursive list  of the directories in a share

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