Use RPC client
---
#rpc #rpc-client #rpcclient

```shell
#enumerate users
rpcclient -U "" -N -c enumdomusers $TARGET_IP
#extract usernames
rpcclient -U "" -N -c enumdomusers $TARGET_IP | grep -o 'user:\[[^]]*\]' | awk -F'[][]' '{print $2}'
```

```shell
rpcclient -U "" $TARGET_IP
```

or with user "guest":

```bash
rpcclient -U guest $TARGET_IP
# lookupsids = sids -> username
enumdomusers
querydispinfo
enumprivs
enumprinters
```
Change user password via RPC
---
use net
```bash
#could be a password, krb5 ccache, no pass, nt hash...
net rpc password $USER -U $OWNED_USER -S $TARGET_IP -P $PASSWORD
```
Impacket RPC dump
---
#impacket #impacket-rpc

```bash
impacket-rpcdump -h
```