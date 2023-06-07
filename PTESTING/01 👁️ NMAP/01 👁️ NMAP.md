#nmap #enumeration #scan

Fastest TCP connect scan with banner grabbing, only on open ports:
---
#enum #nmap #enumeration #scan 
```bash
nmap -p- -sT $TARGET_IP --open -vv -oN $TARGET_NAME.ports.txt
```

Then do:

```bash
export TARGET_PORTS=1,2,3
nmap -Pn -p $TARGET_PORTS -sCV -oN $TARGET_NAME.services.txt $TARGET_IP
```

```bash
nmap $TARGET_IP -Pn --script=/usr/share/nmap/scripts/
```

New arguments to test and compare time:
---

```bash
sudo nmap -p- -sS --open --min-rate 5000 -vvv -n -Pn $TARGET_IP -oG $TARGET_NAME.ports.txt
nmap -p $TARGET_PORTS -sCV $TARGET_IP -oN $TARGET_NAME.services.txt
```

Full port scan with banner grabbing:
---

```bash
nmap -sC -sV -p- oN $TARGET_NAME.ports.txt $TARGET_IP -vv
```

Scan the top 1000 used ports:
---
```bash
nmap $TARGET_IP --top-ports 1000 -sV -sC -vv
```

Faster scan for open ports:
---
```bash
nmap -p- --open --reason -oN $TARGET_NAME.ports.txt $TARGET_IP
```

NMAP ping sweep
---
```bash
nmap -sP TARGET_IP.0/24
```
