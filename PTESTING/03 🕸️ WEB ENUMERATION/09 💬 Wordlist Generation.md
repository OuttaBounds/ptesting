Generate wordlist from web page
---
#wordlist #generate #generation #cewl

Crawl the site page 
```bash
cewl $TARGET_URL > passwords
```

Crawl the site up to depth 5 filter words less than 4 characters:
#crawl #wordlist #generate #cewl

```bash
cewl -w $TARGET_NAME -d 5 -m 4 $TARGET_URL
```
