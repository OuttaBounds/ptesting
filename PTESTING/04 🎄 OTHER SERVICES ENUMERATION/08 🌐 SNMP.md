
Common community SNMP strings:
---
```text
public
private
manager
```

Get community strings:
---
```shell
onesixtyone $TARGET_IP
onesixtyone -c $SNMP_STRINGS_FILE $TARGET_IP
```

Enumerate SNMP:
---
```shell
snmpbulkwalk -v2c -c public $TARGET_IP
```

```shell
snmpwalk -v2c -c public $TARGET_IP
```

using metasploit:

```
msfconsole
```

then:

```metasploit
use auxiliary/scanner/snmp/snmp_enum
set RHOSTS $TARGET_IP
run
```

Get target time and date:

```
snmpget -v2c -c public $TARGET_IP iso.3.6.1.4.1.2021.100.4.0
```

Enumerate entire MIB tree:

```bash
snmpwalk -c public -v1 -t 10 $TARGET_IP
```

Show running software:
```bash
snmpwalk -c public -v1 -t $TARGET_IP 1.3.6.1.2.1.25.6.3.1.2
```

Query installed software:

```bash
snmpwalk -c public -v1 -t $TARGET_IP 1.3.6.1.2.1.25.6.3.1.2
```