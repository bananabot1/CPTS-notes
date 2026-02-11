**Overview:**
 - Attackers frequently exploit legitimate services (like SSH or FTP) to move files quietly within a compromised network.
 - File transfers look simple but are powerful: misconfigured SSH, FTP, NFS or MS‑SQL services let attackers stage payloads, exfiltrate secrets, or pivot to the OS with minimal fuss.
---
## Web Servers

```
sudo python3 -m http.server <port>
```

Starts a Python web server for quick hosting of files. Linux host.

---
## SMB

```
impacket-smbserver -ip <lhost> -smb2support -username <user> -password <password> <share> <path>
```

Starts an Impacket SMB server for quick hosting of files. Linux host.