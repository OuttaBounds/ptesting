SUBDOMAIN ENUMERATION:
---
#subdomain #vhost #sub-domain #sub-domains #WFUZZ

```bash
wfuzz -H "Host: FUZZ.$TARGET_URL" --hc 302,400 -H "User-Agent: PENTEST" -c -z file,"/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt" $TARGET_URL
```

```bash
wfuzz -H "Host: FUZZ.$TARGET_URL" -H "User-Agent: PENTEST" --hh 11947 -c -z file,"/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt" $TARGET_URL
```

GET REQUEST FOR FUZZING PARAMS:
---
#fuzzing #params #WFUZZ 

FOR CMD EXECUTION
```bash
wfuzz -c -z file,"/usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt" --hw 0 $TARGET_URL/$FILE?FUZZ=id
```
for LFI
```bash
wfuzz -c -z file,"/usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt" --hw 0 $TARGET_URL/$FILE?FUZZ=../../../../../../etc/passwd
```
using cookies
```bash
wfuzz --hw 99999 -c -b $COOKIE -w "/usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt" $TARGET_URL/$FILE&FUZZ=$TEST_VALUE
```

POST REQUEST FOR FUZZING PARAMS:
--
#post #post-requests #params 

```bash
wfuzz -d "{\"FUZZ\":\"PARAM_DATA\"}" -H "Host: $TARGET_URL" --hc 422 -H "User-Agent: PENTEST" -c -z file,"/usr/share/seclists/Discovery/Web-Content/combined_words.txt" $TARGET_URL/api/param_url
```

DIRECTORY BRUTEFORCING:
--
#bruteforce #dir #directory #web-directory

```bash
wfuzz -w /usr/share/seclists/Discovery/Web-Content/combined_words.txt --hc 404 $TARGET_URL/FUZZ
```



