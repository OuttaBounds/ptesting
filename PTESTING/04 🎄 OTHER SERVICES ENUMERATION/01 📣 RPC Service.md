Use RPC client
---
#rpc #rpc-client #rpcclient

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