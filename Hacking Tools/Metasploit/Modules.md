The Metasploit framework uses snippets of codes called ***modules*** to perform a specific task, such as scanning or exploiting a target. 
- **Exploits:** These modules are used to exploit a vulnerability or weakness within a system.  
- **Payloads:** These modules are left behind on the target system and enable connection to the system after exploitation.  
    Payloads are divided into three subcategories: *singles*, *stagers*, and *stages*.
- **Auxiliary:** These modules provide capabilities such as scanners, fuzzers, and DoS modules.  
- **Encoders:** These modules re-encode exploits and payloads, enabling them to avoid detection from a system's security defenses.  
    They deny the system the ability to perform tasks such as antivirus and firewall restrictions.
- **Post:** Short for post-exploitation, these modules are used after a system has been exploited.  
- **NOPs:** Short for no operation, these modules halt the system's CPU for a clock cycle.  

#### Starting Metasploit
```
sudo su
msfconsole -q
```
#### Searching for Modules
```
show all
help search
# search <search keyword>:<search term>
search type:exploit
search platform:Windows
search name:ms08-067
info exploit/windows/smb/ms17_010_eternalblue
```