File enumeration:
---
#file #enum #enumeration 

```shell
cat /etc/passwd
sudo -l
id
hostname
cat /etc/issue
cat /etc/os-release
uname -a
ps aux
ip a
routel #or route
ss -anp # or -tunlp / ntplu
cat /etc/iptables/rules.v4
ls -lah /etc/cron*
crontab -l
dpkg -l
```

Enumerate other network adapters:
```bash
ip addr
ip route
```

Find interesting files/dirs:

```bash
find / -writable -type d 2>/dev/null
find / -perm -u=s -type f 2>/dev/null
cat /etc/fstab
lsblk
lsmod
/sbin/modinfo libata
```

Get users with shell:

```shell
cat /etc/passwd | grep sh$
```

Get only usernames:

```sh
cat /etc/passwd | grep sh$ | cut -d: -f1
#OR
cat /etc/passwd | grep sh$ | sed 's/ *:.*//'
```

Get all files with SUID / SGID bit set:

```shell
find / -perm -u=s -type f 2>/dev/null
```

```shell
find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -lsa {} \; 2> /dev/null
```

Find all files owned by current user:

```shell
find / -user $(whoami) 2>/dev/null | grep -v "^/run\|^/proc\|^/sys"
```

All writeable files by current user:

```shell
find / -writable -type f 2>/dev/null | grep -v "^/run\|^/proc\|^/sys"
```

Others:

```bash
env
#Get all env vars
printenv
```

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

List installed software and versions on Ubuntu/Debian
```bash
dpkg -l
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
uname -r
arch
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
getcap -r / 2>/dev/null #or /usr/sbin/getcap
```
---

Files for auto enumeration:
---
#lse #linpeas #enumeration #enum4linux 

LinPEAS:
```shell
wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh
```

Linux Smart Enumeration (LSE):
```
/usr/share/linux-smart-enumeration
```
[https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)

LinEnum.sh:
https://github.com/diego-treitos/linux-smart-enumeration/blob/master/lse.sh
---