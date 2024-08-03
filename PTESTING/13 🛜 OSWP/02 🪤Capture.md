Put card in monitor mode:
---
#monitor-mode
```bash
airmon-ng start wlan0
```
```bash
ifconfig wlan0 down #
iwconfig wlan0 mode monitor
ifconfig wlan0 up
airmon-ng check kill
airodump-ng wlan0
```