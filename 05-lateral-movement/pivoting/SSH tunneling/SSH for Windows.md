**Overview:**
- [Plink](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), short for PuTTY Link, is a Windows command-line SSH tool that comes as a part of the PuTTY package when installed. Similar to SSH, Plink can also be used to create dynamic port forwards and SOCKS proxies.
- [Netsh](https://docs.microsoft.com/en-us/windows-server/networking/technologies/netsh/netsh-contexts) is a Windows command-line tool that can help with the network configuration of a particular Windows system. Here are just some of the networking related tasks we can use 
- We can use `netsh.exe` to forward all data received on a specific port (say 8080) to a remote host on a remote port.
- `Netsh` is used for:
	- `Finding routes`
	- `Viewing the firewall configuration`
	- `Adding proxies`
	- `Creating port forwarding rules`

----
Plink
```
plink -ssh -D 9050 ubuntu@10.129.15.50
```
starts an SSH session between the Windows attack host and the Ubuntu server, and then plink starts listening on port 9050.

Netsh
```
netsh.exe interface portproxy add v4tov4 listenport=8080 listenaddress=10.129.15.150 connectport=3389 connectaddress=172.16.5.25
```
Initiate the ssh connection (explain the flags)
```
netsh.exe interface portproxy show v4tov4
```
verify the port forward 
