
Enumerate with empty / guest credentials:
---
#cme #crackmapexec #smb #shares #samba

Enumerate SAMBA shares without specifying user:
```bash
crackmapexec smb $TARGET_IP -u '' -p '' --shares
```

or using "guest" user

```bash
crackmapexec smb $TARGET_IP -u 'guest' -p '' --shares
```

```bash
crackmapexec smb $TARGET_IP -u '' -p ''
```

Connect to IP to share backups:
---
#smb #samba #share #shares

```bash
smbclient \\\\$TARGET_IP\\backups -U ''
```

```bash
smbclient -L $TARGET_IP -U $TARGET_USER
```

```bash
smbclient  \\\\$TARGET_IP\\Public -N
```

**ENUMERATE SMB shares, users…:**
---
#enum #shares #smb #samba #enum4linux

Can show potential users as well in linux as well

```bash
enum4linux $TARGET_IP
```

Show SMB Shares:
---
#smbmap #smb #shares #samba

```bash
smbmap -H $TARGET_IP -u ""
```

```bash
nbtscan $TARGET_IP -f scan-result.txt
```

**Using NMAP scripts -- not usually useful:**
---
#nmap #samba #smb #enumeration #enum 

```bash
nmap -p 139,445 --script=smb-enum-shares.nse,smb-enum-users.nse $TARGET_IP
```

```bash
nmap -p 139,445 --script=smb-enum-* $TARGET_IP
```

```bash
nmap -sS --script=smb-enum-shares $TARGET_IP
```

```bash
nmap --script=smb-vuln-* $TARGET_IP
```

Get SMB Version:
---
#samba #version #smb 
On terminal 1:
```
sudo tcpdump -s0 -n -i eth0 src $TARGET_IP and port 139 -A -c 10 2>/dev/null > cap.txt
```

On terminal 2:
```bash
smbclient -L $TARGET_IP
```

Parse output:
```bash
cat cap.txt | grep -i "samba\|s.a.m" | tr -d '.' | grep -oP 'UnixSamba.*[0-9a-z]' | tr -d '\n'
```

```bash
cat cap.txt | grep -i "samba\|s.a.m" | tr -d '.' | grep -oP 'Samba.*[0-9a-z]' | tr -d '\n'
```