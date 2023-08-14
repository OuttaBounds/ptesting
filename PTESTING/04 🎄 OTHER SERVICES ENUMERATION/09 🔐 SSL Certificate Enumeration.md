Using openssl
---
```sh
openssl s_client -showcerts -connect $TARGET_IP:$PORT | openssl x509 -noout -text
```

