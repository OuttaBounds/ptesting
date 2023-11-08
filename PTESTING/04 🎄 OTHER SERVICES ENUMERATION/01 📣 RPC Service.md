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
```

Impacket RPC dump
---
#impacket #impacket-rpc

```bash
impacket-rpcdump -h
```