
GOBUSTER:
--
#gobuster #dir #directory #web-directory #web 

```bash
gobuster dir -u $TARGET_URL -w /usr/share/seclists/Discovery/Web-Content/common.txt -t 50 -k -e
```

```bash
gobuster dir -u $TARGET_URL -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 50 -k -e
```

```bash
gobuster dir -k -u $TARGET_URL --wordlist /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt -s 404,403 -b "" --exclude-length 0 -k -e
```

Parameters:

```
add: -x .zip,.php,.txt -=-=-= to enumerate for extensions as well
```

Enum VHOSTS / Subdomains:
---
#vhost #subdomain #sub-domain #sub-domains 

```bash
gobuster vhost -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u $TARGET_URL -t 50 --append-domain -k
```