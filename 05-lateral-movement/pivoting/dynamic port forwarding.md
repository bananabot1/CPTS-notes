**Overview:**
`Port forwarding` is a technique that allows us to redirect a communication request from one port to another. Port forwarding uses TCP as the primary communication layer to provide interactive communication for the forwarded port. However, different application layer protocols such as SSH or even [SOCKS](https://en.wikipedia.org/wiki/SOCKS) (non-application layer) can be used to encapsulate the forwarded traffic. This can be effective in bypassing firewalls and using existing services on your compromised host to pivot to other networks.
The Nmap output shows that the SSH port is open. To access the MySQL service, we can either SSH into the server and access MySQL from inside the Ubuntu server, or we can port forward it to our localhost on port `1234` and access it locally. A benefit of accessing it locally is if we want to execute a remote exploit on the MySQL service, we won't be able to do it without port forwarding. This is due to MySQL being hosted locally on the Ubuntu server on port `3306`

```
ssh -L 1234:localhost:3306 ubuntu@10.129.202.64
```
The `-L` command tells the SSH client to request the SSH server to forward all the data we send via the port `1234` to `localhost:3306` on the Ubuntu server.
```
ssh -L 1234:localhost:3306 -L 8080:localhost:80 ubuntu@10.129.202.64
```
forwarding multiple ports

setting up a pivoting port
```
ssh -D 9050 ubuntu@10.129.202.64
```
enabling port forwarding with ssh.
The `-D` argument requests the SSH server to enable dynamic port forwarding. Once we have this enabled, we will require a tool that can route any tool's packets over the port `9050`. We can do this using the tool `proxychains`, which is capable of redirecting TCP connections through TOR, SOCKS, and HTTP/HTTPS proxy servers

```
```
proxychains nmap -v -Pn -sT 172.16.5.19
```
```
scan the host through the pivot

```
proxychains msfconsole
```
open Metasploit using proxychains and send all associated traffic through the proxy we have established.
```
proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```
