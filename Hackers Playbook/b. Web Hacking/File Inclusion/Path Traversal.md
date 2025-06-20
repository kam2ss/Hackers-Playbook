Also known as **Directory Traversal**, is a web vulnerability that lets attackers access files on the server by manipulating URLs. This allows them to read files outside the application's root directory.

This issue happens when user input is passed to functions like `file_get_contents` in PHP. This vulnerability is usually caused by poor input validation, not the function itself.

**Example:**
	If the attacker finds the entry point, which in this case `get.php?file=`, then the attacker may send something as follows, `http://webpage.thm/get.php?file=../../../etc/passwd`

Below are some common OS files you could use when testing: 

|   |   |
|---|---|
|**Location**|**Description**|
|/etc/issue|contains a message or system identification to be printed before the login prompt.|
|/etc/profile|controls system-wide default variables, such as Export variables, File creation mask (umask), Terminal types, Mail messages to indicate when new mail has arrived|
|/proc/version|specifies the version of the Linux kernel|
|/etc/passwd|has all registered user that has access to a system|
|/etc/shadow|contains information about the system's users' passwords|
|/root/.bash_history|contains the history commands for root user|
|/var/log/dmessage|contains global system messages, including the messages that are logged during system startup|
|/var/mail/root|all emails for root user|
|/root/.ssh/id_rsa|Private SSH keys for a root or any known valid user on the server|
|/var/log/apache2/access.log|the accessed requests for Apache  webserver|
|C:\boot.ini|contains the boot options for computers with BIOS firmware|

---

#### Base Directory Breakouts
**Breakout Example:**
`?page=/var/www/html/..//..//..//etc/passwd`

#### Obfuscation
Obfuscation techniques are often used to bypass basic security filters that block directory traversal attempts `e.g., ../`.

**Common Techniques**
- **URL Encoding Bypass:** URL-encoded version of the payload `?file=%2e%2e%2fconfig.php`. The server decodes this input to `../config.php`, bypassing the filter.
- **Double Encoding Bypass:** Encodes the input twice to bypass double-encoding filters, `?file=%252e%252e%252fconfig.php ` translates to `../config.php`.
- **Obfuscation:** `....//config.php` becomes `../config.php`.
