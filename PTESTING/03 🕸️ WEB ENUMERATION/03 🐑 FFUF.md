#FFUF #enumeration 

**FFUF DIR bruting:**
---
#dir #directory #web-directory 

```bash
ffuf -u $TARGET_URL/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt
```

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt -u $TARGET_IP -recursion -e *.txt,.php,.html,.bak,.jar,.war,.backup,._backup -ac
```
**FFUF post request:**
---
#post #post-requests #api

```bash
ffuf -u $TARGET_URL/FUZZ -X POST -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories-lowercase.txt -mc all -fs 50 #-fs - filter results with file size equal to 50
```

**FFUF VHOST / SUBDOMAIN Bruting:**
---
#subdomain #vhost #bruteforce 

```bash
ffuf -u http://$TARGET_IP -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -H "Host: FUZZ.$TARGET_URL" -ac
#don't forget HTTPS
```

```bash
ffuf -u http://$TARGET_URL -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -H "Host: FUZZ.$TARGET_URL" -fs #default size
```

**FFUF Brute force 2FA TOTP code auth:**
---
#2fa #TOTP #bruteforce 

```bash
ffuf -w /usr/share/seclists/Fuzzing/6-digits-000000-999999.txt -request $BURPREQUEST -t 100
```

**FFUF Parameter fuzzing:**
---
#params #api #bruteforce 

```bash
ffuf -u $TARGET_URL/$FILE.php?FUZZ=/etc/passwd -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -ac
```

FFUF Basic Authentication Auth
---
#basic-auth #basic #auth 
Generate header using:
```
export WUSER=$USER
export WPASS=$PASS
ffuf -u http://$TARGET_IP/FUZZ -w $WDIRS -H "Authorization: Basic $(echo $WUSER:$WPASS | base64)" -ac
```

FFUF File extensions(i.e. .php, .txt...):
---
#extensions #file-extensions

```bash
ffuf -u $TARGET_URL/FUZZ -w $WFILES -e .php,.txt,.html,.pdf,.zip,.log,config -ac
```

**Extended Common Wordlists:**
---
#wordlists #web

```bash
/usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
```

```
/usr/share/seclists/Discovery/Web-Content/combined_directories.txt
```

```
/usr/share/seclists/Discovery/Web-Content/combined_words.txt
```

```
/usr/share/seclists/Discovery/Web-Content/raft-small-words.txt
```

```
/usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt
```
