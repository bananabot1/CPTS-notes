**Overview:**
- A subdomain is any website underlying another domain (e.g., photos.google.com is a subdomain of google.com).
- VHosts (Virtual Hosts) are subdomains served on the same server with the same IP — a single IP can serve multiple different websites.
- Key difference: subdomains have DNS records, VHosts may only exist as Host header configurations without public DNS entries.
---
## DNS Configuration

```
sudo sh -c 'echo "<target-ip> <domain>" >> /etc/hosts'
```

Adds DNS entry to resolve domain locally.

---
## Subdomain Fuzzing

```
ffuf -w <wordlist>:FUZZ -u https://FUZZ.<domain>/
```

Discovers public subdomains via DNS resolution.

---
## VHost Fuzzing

```
ffuf -w <wordlist>:FUZZ -u http://<domain>:<port>/ -H 'Host: FUZZ.<domain>' -fs <size>
```

Discovers virtual hosts by fuzzing the Host header. `-fs` filters responses by size to hide false positives.