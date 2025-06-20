Repeater is a tool in Burp Suite used for ***manually modifying and resending HTTP or WebSocket requests and then reissue them to the server***. It is especially ***useful for testing*** how specific inputs affect application behaviour by sending the same request repeatedly with slight changes. It also helps further investigate requests flagged as suspicious by other Burp tools, offering a controlled way to probe for input-based weakness.

**Uses**
Example, an attacker could leverage Burp Repeater's capabilities to help ***detect*** and ***perform*** `parameter manipulation` via ***SQL injection*** and/or ***XSS*** vulnerabilities. This can be achieved with the ***reissuing of requests again and again***, each one altered slightly until the desired result is achieved. 

### Using Repeater
In the Repeater tab, you can:
- Click `Go` to send the request and view the response.
- Modify parameters via the editor or Params tab before resending.
- Open a new Repeater tab manually and choose HTTP.
- Follow redirects with a provided button and navigate request history using arrows.
- View and edit parameters directly in the Params tab or request body.

### Attack Options
The Repeater menu can control some aspects of the tool's behaviour. Following options are available:
- `Auto-Update Content-Length:` ensures the ***header updates when modifying request bodies***.
- `Auto-Decompress Responses:` automatically ***unpacks GZIP or deflate-compressed content***.
- `Follow Redirects:` automatically follow redirect responses or prompts you if disabled.
- `Handle Cookies on Redirects:` resubmits cookies from redirection responses when following them
