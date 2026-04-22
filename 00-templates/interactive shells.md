**Overview:**
When we drop into the system shell, we notice that no prompt is present, yet we can still issue some system commands. This is a shell typically referred to as a `non-tty shell`. These shells have limited functionality and can often prevent our use of essential commands like `su` (`switch user`) and `sudo` (`super user do`), which we will likely need if we seek to escalate privileges.
It is possible to manually spawn a TTY bourne shell to give us access to more commands and a prompt to work from

```
python -c 'import pty; pty.spawn("/bin/sh")' 
```
```
/bin/sh -i
```
```
perl -e 'exec "/bin/sh";'
```
```
ruby: exec "/bin/sh"
```
```
lua: os.execute('/bin/sh')
```
```
awk 'BEGIN {system("/bin/sh")}'
```
vim -c ':!/bin/sh'
```
```
vim
:set shell=/bin/sh
:shell
```