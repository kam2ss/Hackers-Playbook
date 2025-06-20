[[RootShell Playbook/1. Recon/Nmap|Nmap]](Nmap scans from basic to stealthy from the `RootShell playbook`.)

### Universal options from - Basic to Advanced
| Option                | Purpose                                  |
| --------------------- | ---------------------------------------- |
| -p-                   | all ports                                |
| -p1-1023              | scan ports 1 to 1023                     |
| -F                    | 100 most common ports                    |
| -r                    | scan ports in consecutive order          |
| -T<0-5>               | -T0 being the slowest and T5 the fastest |
| --max-rate 50         | rate <= 50 packets/sec                   |
| --min-rate 15         | rate >= 15 packets/sec                   |
| --min-parallelism 100 | at least 100 probes in parallel          |

### Basic Commands
| Port Scan Type   | Example Command             |                                                                               |
| ---------------- | --------------------------- | ----------------------------------------------------------------------------- |
| TCP Connect Scan | nmap -sT 10.10.222.244      | `No Root required`, `completes full handshake`                                |
| TCP SYN Scan (   | sudo nmap -sS 10.10.222.244 | `Root required`, `sends only SYN`, **may be blocked by firewall**             |
| UDP Scan         | sudo nmap -sU 10.10.222.244 | **ICMP port unreachable error (type 3, code 3) is returned on a closed port** |
