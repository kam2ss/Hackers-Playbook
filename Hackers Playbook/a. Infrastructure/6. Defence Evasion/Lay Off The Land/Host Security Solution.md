### Antivirus Software
Antivirus software is mainly used to ***monitor, detect, and prevent malicious software*** from being executed within the host.

**Various detections techniques that antivirus use:**
- **Signature-based detection:** Often, researchers or users submit their infected files into an antivirus engine platform for further analysis by AV vendors, and if it confirms as malicious, then the signature gets registered in their database. The antivirus compares the scanned file with a database of known signatures.
- **Heuristic-based detection:** It can statically analyze in real-time by ***decompiling*** a suspected program and examining its source code and compare the pattern against a heuristic database. It also dynamically ***runs suspicious code in a sandbox*** (virtual machine) to observe behaviour safely.
- **Behaviour-based detection:** It ***relies on monitoring and examining the execution of applications*** to find abnormal behaviours and uncommon activities such as creating/updating values in registry keys, killing/creating processes, etc.

**Check if an antivirus is present or not:**
**wmic**
```
wmic /namespace:\\root\securitycenter2 path antivirusproduct
```

**PowerShell**
```
Get-CimInstance -Namespace root/SecurityCenter2 -ClassName AntivirusProduct
```

---
### Microsoft Windows Defender
Microsoft Windows Defender is a pre-installed antivirus that works in three modes:
- **Active:** runs as the ***primary antivirus software***.
- **Passive:** runs when a ***third-party antivirus software is installed***.
- **Disable:** if ***disabled or uninstalled*** from the system.

**Check the service state of Windows Defender:**
```
Get-Service WinDefend
```

Using the `Get-MpComputerStatus` cmdlet to get the current Windows Defender status. However, it provides the current status of security solution elements, including `Anti-Spyware`, `Antivirus`, `LoavProtection`, `Real-time protection`, etc. We can use select to specify what we need for as follows,
```
Get-MpComputerStatus | select RealTimeProtectionEnabled
```

---
### Host-Based Firewall
The main purpose of the host-firewall is to ***control the inbound and outbound traffic*** that goes through the device's interface. 

**Check Firewall Status:**
```
Get-NetFirewallProfile | Format-Table Name, Enabled
```

**Disable Firewall, if we have admin privileges:**
```
Set-NetFirewallProfile -Profile Domain, Public, Private -Enabled False
```

**Check the Current Firewall rules:**
```
Get-NetFirewallRule | select DisplayName, Enabled, Description
```

**During a red team engagement, if the firewall's rules are unknown, built-in PowerShell cmdlets can be used to test inbound connections without extra tools:**
```
Test-NetConnection -ComputerName 127.0.0.1 -Port 80
```
We can confirm port 80 is open and allowed in the firewall. Remote targets or domain names can also be tested using the `-ComputerName` argument with `Test-NetConnection.`

---
### Security Event Logging and Monitoring
By default, OS log various activity events in the system using log files. In addition, security and network devices store event information into log files to allow the administrators to get an insight into what is going on.

**List available event logs**
```
Get-EventLog -List
```

---
### System Monitor (Sysmon)
Windows System Monitor `sysmon` is a service and device driver. It is one of the Microsoft `Sysinternals` suites. The `sysmon` tool is not an essential tool (not installed by default), but it starts gathering and logging evets once installed.

This tool can log many important events, and you can also create your own rule(s) and configuration to monitor.

**Look for the Process name `sysmon`**
```
Get-Process | Where-Object { $_.ProcessName -eq "Sysmon" }
```

**Look for the Service `System Monitor Service`
```
Get-CimInstance win32_service -Filter "Description = 'System Monitor Service"

# or

Get-Service | Where-Object { $_.DisplayName -like "*sysm*" }
```

---
### Host-based Intrusion Detection/Prevention System (HIDS/HIPS)
**HIDS**
It is a software that ***monitor and detect abnormal and malicious activities*** in a host. The primary purpose is to ***detect and not to prevent them***. Methods how detection works:
- **Signature-Based IDS:** Looks at checksums and message authentication.
- **Anomaly-Based IDS:** Looks for unexpected activities, including abnormal bandwidth usage, protocols, and ports.

**HIPS**
It secure the OS system activities of the device where they are installed. It is a detection and prevention solution against well-known attacks and abnormal behaviours. HIPS can audit the host's log files, monitor processes, and protect system resources. HIPS combines many product features such as antivirus, behaviour analysis, network, application firewall, etc.

---
### Endpoint Detection and Response (EDR)
It is also known as Endpoint Detection and Threat Response (EDTR). EDRs can look for malicious files, monitor endpoints, system, and network events, and record them in a database for further analysis, detection, and investigation. EDRs are the next generation of antivirus and detect malicious activities on the host in real-time. 

EDR analyze system data and behavior for making section threats, including
- Malware, including viruses, trojans, adware, keyloggers
- Exploit chains
- Ransomware

Below are some common EDR software for endpoints
- `Cylance`
- `Crowdstrike`
- `Symantec`
- `SentinelOne`
- Many others

Even though an attacker successfully delivered their payload and bypassed EDR in receiving reverse shell, EDR is still running and monitors the system. It may block us from doing something else if it flags an alert.