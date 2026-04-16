**Overview:**
- [Chisel](https://github.com/jpillora/chisel) is a TCP/UDP-based tunneling tool written in Go that uses HTTP to transport data that is secured using SSH. `Chisel` can create a client-server tunnel connection in a firewall restricted environment.
- In a scenario attackers might have to tunnel traffic to a webserver on the `172.16.5.0`/`23` network (internal network). The Domain Controller with the address `172.16.5.19`. 
- This is not directly accessible to the attack host since attack host and the domain controller belong to different network segments. However, since an Ubuntu server has already been compromisded, it allows to start a Chisel server on it that will listen on a specific port and forward traffic to the internal network through the established tunnel.
---
```
git clone https://github.com/jpillora/chisel.git
```
cd chisel
go build
```
```
installation 

```
scp chisel ubuntu@10.129.202.64:~/
```
trasnfer to target

```
./chisel server -v -p 1234 --socks5
```
run the server on the target host

The Chisel listener will listen for incoming connections on port `1234` using SOCKS5 (`--socks5`) and forward it to all the networks that are accessible from the pivot host

```
./chisel client -v 10.129.202.64:1234 socks
```
start a client from the attack host and connect to the target server.

, the Chisel client has created a TCP/UDP tunnel via HTTP secured using SSH between the Chisel server and the client and has started listening on port 1080. 
```
 tail -f /etc/proxychains.conf 

#
#       proxy types: http, socks4, socks5
#        ( auth types supported: "basic"-http  "user/pass"-socks )
#
[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
# socks4    127.0.0.1 9050
socks5 127.0.0.1 1080
```
modify our proxychains.conf file located at `/etc/proxychains.conf` and add `1080` port at the end
