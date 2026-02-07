"locate" -- built-in database in Kali. 
"which"

EXPLOIT DB / SearchSploit:

Find exploit for an application:
#searchsploit
```shell
searchsploit $APP_NAME
```

Copy exploit to current dir:
```shell
searchsploit -m $EXPLOIT_ID
```

WORDLISTS:
#rockyou #seclists
```
/usr/share/wordlists/rockyou.txt
```

```
/usr/share/seclists/…
```

Create wordlist of all 6 digit combinations from 000000 to 999999

```shell
crunch 6 6 0123456789 -o wordlist.txt to generate wordlist
```
