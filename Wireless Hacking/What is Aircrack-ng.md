### Introduction
**Aircrack-ng** is a suite of tools used to assess Wi-Fi network security. It focuses on four main areas of security:
- **Monitoring:** Capture network packets and export any collected data to third party tools (Wireshark).
- **Attacking:** Attack Wi-Fi networks with, such as `deauthenticating` connected devices, running `ARP replay` attacks, and `creating fake access points`. All of these attacks are types of packet injection.
- **Testing:** Test vulnerabilities by checking Wi-Fi cards and driver capabilities.
- **Cracking:** Crack the encryption keys of WEP and WPA/WPA2 networks.

### Aircrack suite
**Airmon-ng**
It ***toggles your network interface card (NIC) between modes***, primarily `managed` and `monitor` mode. 
- `Managed mode:` NIC processes only packets intended for it—like a private conversation between you and a friend.
- `Monitor mode:` NIC captures _all_ network traffic around it, even if not meant for the device—like overhearing others’ conversations while walking by.
- `Promiscuous mode:` NIC captures all packets on the connected network regardless of the destination MAC address—like overhearing group conversations not meant for you.
After switching to ***monitor mode***, you can use ***airodump-ng*** to sniff and capture packets.

**Airodump-ng**
It is the ***packet sniffer and capture tool*** with Aircrack-ng. It intercepts and logs all wireless traffic within range, listing nearby networks and their access points. 
- `Stations:` These are the devices connected to networks are also identified by airodump-ng.
- Airodump-ng captures detailed info about networks, including channel, MAC address, BSSID, etc.
- You can connect to a target and access point using airodump-ng.
- It creates capture files storing key network data for use in packet injection attacks with **aireplay-ng**.

**Aircrack-ng**
It is the key cracking tool in the Aircrack suite for WEP and WPA/WPA2-PSK keys.
- `WEP:` keys are typically cracked using the `PTW method`.
- `WPA/WPA2:` keys are cracked with `dictionary attacks.`
- Aircrack-ng relies on ***packet capture data*** for WEP cracking.
- For WPA/WPA2, it requires the 4-way hand
