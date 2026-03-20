**Overview:**
`LDAP` (Lightweight Directory Access Protocol) is `a protocol` used to `access and manage directory information`. A `directory` is a `hierarchical data store` that contains information about network resources such as `users`, `groups`, `computers`, `printers`, and other devices.
LDAP is `commonly used` for providing a `central location` for `accessing` and `managing` directory services. Directory services are collections of information about the organisation, its users, and assets–like usernames and passwords. LDAP enables organisations to store, manage, and secure this information in a standardised way.
There are two popular implementations of LDAP: `OpenLDAP`, an open-source software widely used and supported, and `Microsoft Active Directory`, a Windows-based implementation that seamlessly integrates with other Microsoft products and services.

Although LDAP and AD are `related`, they `serve different purposes`. `LDAP` is a `protocol` that specifies the method of accessing and modifying directory services, whereas `AD` is a `directory service` that stores and manages user and computer data.
LDAP works by using a `client-server architecture`. A client sends an LDAP request to a server, which searches the directory service and returns a response to the client.

---
enumeration
```
ldapsearch -H ldap://ldap.example.com:389 -D "cn=admin,dc=example,dc=com" -w secret123 -b "ou=people,dc=example,dc=com" "(mail=john.doe@example.com)"
```
`ldapsearch` is a command-line utility used to search for information stored in a directory using the LDAP protocol.
- Connect to the server `ldap.example.com` on port `389`.
- Bind (authenticate) as `cn=admin,dc=example,dc=com` with password `secret123`.
- Search under the base DN `ou=people,dc=example,dc=com`.
- Use the filter `(mail=john.doe@example.com)` to find entries that have this email address.

