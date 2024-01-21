
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

```bash
msfconsole -x "use auxiliary/scanner/snmp/snmp_enum; set RHOSTS $TARGET_IP;run;"
```

Get target time and date:

```
snmpget -v2c -c public $TARGET_IP iso.3.6.1.4.1.2021.100.4.0
```

Enumerate entire MIB tree:

```bash
snmpwalk -c public -v1 -t 10 $TARGET_IP
```

Show software running on the machine:
```bash
snmpwalk -c public -v1 -t $TARGET_IP 1.3.6.1.4.1.77.1.2.25
```

Query what software is installed on the machine:

```bash
#-Oa converts hexademical -> string
snmpwalk -c public -v1 -t 1.3.6.1.2.1.25.6.3.1.2 $TARGET_IP 
```


|ID|NAME|
|------------------------|------------------|
| 1.3.6.1.2.1.25.1.6.0   | System Processes | 
| 1.3.6.1.2.1.25.4.2.1.2 | Running Programs |
| 1.3.6.1.2.1.25.4.2.1.4 | Processes Path   |
| 1.3.6.1.2.1.25.2.3.1.4 | Storage Units    |
| 1.3.6.1.2.1.25.6.3.1.2 | Software Name    |
| 1.3.6.1.4.1.77.1.2.25  | User Accounts    |
| 1.3.6.1.2.1.6.13.1.3   | TCP Local Ports  |

#snmp-running-programs
```bash
snmpwalk -v2c -c public $TARGET_IP 1.3.6.1.2.1.25.4.2.1.2
```