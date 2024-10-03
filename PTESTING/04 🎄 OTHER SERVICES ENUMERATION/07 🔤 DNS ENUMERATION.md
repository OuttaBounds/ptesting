Get All resolves for local DNS Server:
---
#dig #dns #zone #zone-transfer 

```shell
dig @$TARGET_IP -x $TARGET_IP
```

```shell
dig $TARGET_IP
```

```shell
dig any $TARGET_DOMAIN @$TARGET_IP
```

zone transfer:
```shell
dig axfr $TARGET_DOMAIN @$TARGET_IP
```
---
---
```bash
subfinder -d $TARGET_DOMAIN -all -cs | tee domains.unf.txt | cut -d "," -f 1 > domains.txt
```
Add subdomain to a DNS zone:
---
#nsupdate #subdomain #add-domain

Use "nsupdate" to generate config and talk to DNS server:

```shell
nsupdate
```
and then:
```nsupdate
server $TARGET_IP 53
key hmac-sha256:rndc-key $KEY
zone $TARGET_DOMAIN
update add $SUBDOMAIN.$TARGET_DOMAIN 86400 A $ATTACKER_IP
send
```
---
Zone Transfer using host:
---
#zone #transfer #zone-transfer #dns 

```shell
host -l $TARGET_DOMAIN $TARGET_IP
```
---