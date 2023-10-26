Use RPC client
---
#rpc #rpc-client #rpcclient

```shell
#enumerate users
rpcclient -U "" -N -c enumdomusers $TARGET_IP
#extract usernames
grep -o 'user:\[[^]]*\]' users | awk -F'[][]' '{print $2}'
```

```shell
rpcclient -U "" $TARGET_IP
```

or with user "guest":

```bash
rpcclient -U guest $TARGET_IP
# lookupsids = sids -> username
```

Impacket RPC dump
---
#impacket #impacket-rpc

```bash
impacket-rpcdump -h
```