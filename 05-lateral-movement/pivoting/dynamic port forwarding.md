**Overview:**
- Port forwarding redirects communication requests from one port to another, using TCP as the primary transport. SSH or SOCKS can be used to encapsulate forwarded traffic, effectively bypassing firewalls and leveraging existing services on a compromised host to pivot into other networks.
- Local port forwarding allows accessing services on a remote host as if they were running locally, enabling exploitation of services that would otherwise be unreachable from the attack machine.
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

```
proxychains xfreerdp /v:<target> /u:<user> /p:<password>
```

Connect to RDP through the pivot.```
