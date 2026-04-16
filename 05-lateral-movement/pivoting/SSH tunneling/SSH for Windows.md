**Overview:**
- [Plink](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), short for PuTTY Link, is a Windows command-line SSH tool that comes as a part of the PuTTY package when installed. Similar to SSH, Plink can also be used to create dynamic port forwards and SOCKS proxies.


----

```
plink -ssh -D 9050 ubuntu@10.129.15.50
```
starts an SSH session between the Windows attack host and the Ubuntu server, and then plink starts listening on port 9050.
