### Network Firewall
The firewall ***filters the untrusted traffic*** before passing it into the network based on rules and policies. It can be used to separate networks from external traffic sources, internal traffic sources, or even specific applications. The following are some firewall types:
- **Packet-filtering firewalls:** Inspect and filter based on predefined security rules such as IP addresses, port, and protocol.
- **Proxy firewalls:** Act as an intermediary between users and the internet, filtering traffic and hiding user's IP for privacy.
- **NAT firewalls:** masking private internal IP with a public one to protect internal networks.
- **Web application firewalls:** monitor and filter HTTP traffic to protect attacks like SQL injection and XSS.
- **NGFW (Next Generation Firewall):** combines traditional firewall features with advanced security functions like deep packet inspection, threat intelligence etc.

### Security Information and Event Management (SIEM)
SIEM combines Security Information Management (SIM) and Security Event Management (SEM) to monitor and analyze events and track and log data in real-time.

It is a security solution that collects, analyses, and correlates security event data from multiple sources in real-time to detect, respond to, and report on potential security threats. It is capable of detecting advanced and unknown threats using integrated threat intelligence and AI technologies like DDoS, etc.

SIEM products:
- **Splunk**
- **SolarWinds Security Event Manager**

### Intrusion Detection System and Intrusion Prevention System (NIDS/NIPS)
Network-based IDS/IPS have a similar concept to the host-based IDS/IPS. The main difference is that the network-based products focus on the security of a network instead of a host.

Common enterprise IDS/IPS products:
- **Palo Alto Networks**
- **Suricata**
