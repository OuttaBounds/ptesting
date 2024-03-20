DIRB CMDs:
---
```bash
dirb $TARGET_URL
```

Append each word with .txt:
---
```bash
dirb $TARGET_URL -X .txt
```

Web project config files:
---
```
Maven: pom.xml
```

```
npm/node: package.json
```

NIKTO:
---
```bash
nikto -h $TARGET_URL
```

Whatweb:
---
#detect-web-apps
```bash
whatweb http://$TARGET_IP
```