IDOR stands for Insecure Direct Object Reference and is a type of access control vulnerability.

This type of vulnerability where ***users can access or modify unauthorized data by changing parts of a URL or request***. It happens when a server trusts user input without verifying if the user is allowed to access the requested object.

**Example:**
	If you have a link like `example.com/profile/123,` where `123` refers to the ***user ID***, an ***attacker might change*** that to `example.com/profile/1000` and ***access someone else's profile*** without permission.