Most web servers, by default, will return their name and version inside the headers of every HTTP response. 

#### Apache
In its default configuration, Apache exposes information about the server.
	Two settings that affect this:
		**ServerTokens**: defines the level of information to provide inside a server signature. 
		```Server: Apache/2.4.2 (Unix) PHP/7.1```
		**ServerSignature**: defines a Boolean (On/Off) that indicate if Apache should show server tokens on server-generated documents.
			For e.g.  visit ``/web/js`` to see a directory listing
				Configuration file for older installations of Apache:
					``/etc/apache2/apache.conf``
				Modern configuration:
					``/etc/apache2/conf-enabled/security.conf``
					Change the signature and tokens here:
						```ServerSignature Off
						ServerTokens Prod```

For question 5. in Apache - Server Details, check the developer tools.
	It shows the Server: Apache, which is the answer.
	 