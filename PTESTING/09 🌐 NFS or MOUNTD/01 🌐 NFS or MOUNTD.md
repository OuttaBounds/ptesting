Open port 893:

```shell
mkdir /tmp/mount
showmount -e $TARGET_IP
```

```shell
sudo mount -t nfs $TARGET_IP:/PATH_TO_INTERESTING_MOUNT /tmp/mnt
```