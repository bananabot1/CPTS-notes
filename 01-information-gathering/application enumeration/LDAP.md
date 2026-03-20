**Overview:**
- `LDAP` (Lightweight Directory Access Protocol) is a protocol used to access and manage directory information. A `directory` is a hierarchical data store containing information about network resources such as users, groups, computers, printers, and other devices.
- LDAP is commonly used to provide a central location for accessing and managing directory services, collections of information about the organisation, its users, and assets like usernames and passwords.
- `OpenLDAP` (open-source) and `Microsoft Active Directory` (Windows-based, integrates with Microsoft products).
- Although LDAP and AD are related, they serve different purposes. `LDAP` is a protocol that specifies how to access and modify directory services, whereas `AD` is a directory service that stores and manages user and computer data. LDAP works via a client-server architecture a client sends a request to a server, which searches the directory and returns a response.

---
## Enumeration

```
nmap -p- -sC -sV --open --min-rate=1000 <target>
```

Identify LDAP on port `389` (or `636` for LDAPS).

```
ldapsearch -H ldap://<target>:389 -D "cn=<user>,dc=<domain>,dc=<tld>" -w <password> -b "ou=<ou>,dc=<domain>,dc=<tld>" "(<filter>)"
```

Query the LDAP directory. `-H` target URI, `-D` bind DN (authenticate as), `-w` password, `-b` base DN to search under, last argument is the search filter (e.g. `(mail=john.doe@example.com)`.```

