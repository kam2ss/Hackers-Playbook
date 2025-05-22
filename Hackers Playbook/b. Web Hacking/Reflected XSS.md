### Reflected cross-site scripting (XSS)
Reflected XSS takes place when an application receives user input and then sends it back in a later HTTP response. Reflected XSS attacks occur immediately, as the data is echoed into the response.

If the application doesn't ***sanitize*** the input data, attackers can exploit this vulnerability by injecting malicious scripts into the HTTP response. 
![An example of reflected XSS in a web application. The web application gets the name field from the user input. The payload sent to the application is <script>alert("XSS")</script>. When the page is rendered, an alert box with "XSS" displayed is shown.](https://il-labforge-assets.origin.immersivelabs.team/uploads/RjHrUDBKzlu_4eGbQq1AgqOP-wRFhsPHqaovmBJccGo.png)

### Exploiting reflected XSS
The response from the server includes the ***unsanitised*** search query, making it vulnerable to reflected XSS attacks. 
![A student records application. The user has searched for the ID number 0206, which is shown in a URL query string. This ID number is also shown on the page.](https://il-labforge-assets.origin.immersivelabs.team/uploads/kYB-aTT1CoxdrUhJCEyKMs4TeCV8YTnncjo60Dlv89k.png)
When the ID number is directly echoed back in the response without sanitization or processing, an attacker can inject malicious scripts into the web application's response.

If an attacker were to submit this payload to the application, the script would be injected into the response instead of the expected ID number:
```html
<p> Displaying results for ID: <script>alert('xss')</script></p>
```

Reflected XSS attacks can also come from ***POST*** requests, not just ***GET*** requests (as demonstrated above). In this instance, user input might be submitted through a form and sent via a POST request to the server, which uses the data to generate HTTP responses. 

### AI Breakdown
Here’s a step-by-step breakdown:
1. The user (or an attacker) sends a request to the server. This request includes some input from the user. In the case of a Reflected XSS attack, this input includes a malicious script and is typically included in a URL as part of a GET request.
2. The server receives this request and processes it. As part of this processing, the server includes the user’s input in the HTTP response it generates. If the server does not properly sanitize this input, the malicious script will be included in the response.
3. The server sends this HTTP response back to the user. The user’s browser then renders this response. If the response includes a malicious script, the browser will execute this script.

So, the server is indeed making a GET request to the server, and the server is indeed sending back an HTTP response that includes the user’s input. The key to a Reflected XSS attack is that the user’s input is reflected - that is, sent back without being properly sanitized - in the HTTP response. This is why it’s called a “Reflected” XSS attack.