**Overview:**
- Enumerate the internal network, identifying hosts, critical services, and potential avenues for a foothold.
- This can include active and passive measures to identify users, hosts, and vulnerabilities to take advantage to further  access.
---
## DNS Resolution 

```
nslookup <domain>
```

Queries DNS to discover IP ↔ domain mappings.

---
## Network Poisoning

```
sudo responder -I <network-interface>
```

Captures LLMNR, NBT-NS, and MDNS queries on the interface.

---
## Host Discovery

```
fping -asgq 172.16.5.0/23
```

Ping sweep.  Returns only live hosts. `-a` alive, `-s` stats, `-g` CIDR expand, `-q` quiet.

---
## Kerberos Enumeration

```
kerbrute userenum -d <domain> --dc <dc> <wordlist> -o <out-file>
```

Brute-forces valid usernames against a Kerberos domain controller. `-d` domain, `--dc` DC IP, `-o` output.
