Get community strings:
---
```shell
onesixtyone $TARGET_IP
```

Enumerate SNMP:
---
```shell
snmpbulkwalk -v2c -c public $TARGET_IP
```

```shell
snmpwalk -v2c -c public $TARGET_IP
```

```
msfconsole
use auxiliary/scanner/snmp/snmp_enum
set RHOSTS 10.13.37.11
run
```

Get target time and date
```
snmpget -v2c -c public $TARGET_IP iso.3.6.1.4.1.2021.100.4.0
```