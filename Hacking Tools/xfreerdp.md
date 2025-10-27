```
xfreerdp /v:<Workstation-01 IP> /u:<username> /d:<domain> +clipboard +drives /drive:root,/home/kali /dynamic-resolution
```

- `+clipboard`  
    Enables clipboard redirection between local and remote. With `+clipboard` you can copy/paste text (and sometimes files depending on server client) between machines. The `+` means enable; `-clipboard` would disable.

- `+drives`  
    Enables drive redirection support in general (allows individual drives specified with `/drive:` to be redirected). Some builds treat `/drive:` alone as enough, but `+drives` ensures the drive redirection subsystem is active.

- `/drive:root,/home/kali`  
    This maps a local folder into the RDP session as a redirected drive. Format: `/drive:<name>,<local-path>`.
    
    - `<name>` is how the remote Windows session will see it (here `root`).
    
    - `<local-path>` is the path on your Linux box (`/home/kali`).  
        On the Windows side the redirected folder will appear under “This PC” as something like `root (\\tsclient\root)` or similar, letting you read/write files from the remote desktop.