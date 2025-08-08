[www.hashes.org](http://www.hashes.org)
[https://crackstation.net/](https://crackstation.net/)
[https://hashes.com/en/decrypt/hash](https://hashes.com/en/decrypt/hash)
[https://md5.gromweb.com/?md5=87e6d56ce79af90dbe07d387d3d0579e](https://md5.gromweb.com/?md5=87e6d56ce79af90dbe07d387d3d0579e)


```shell
john -w=/usr/share/wordlists/rockyou.txt hashes
```

Other formats:
```bash
#like: ':HASH$SALT'
#or format hash like $dynamic_XX$HASH$SALT
john --list=subformats
john --format=$FORMAT --wordlist=/usr/share/wordlists/rockyou.txt hashes
```

```bash
hashcat -m $MODE_NO hashes /usr/share/wordlists/rockyou.txt
```

Get MD5 hash of $STRING
---
```shell
echo -n $STRING  | md5sum
```
Crack keepass ( .kdbx ) database files:
---
```bash
keepass2john Database.kdbx > hashes
#remove everything up to and includign the first ":" ie. "Database:"
hashcat -m 13400 hashes /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/rockyou-30000.rule --force
```