### Stored cross-site scripting (XSS)
Stored XSS (also known as ***persistent*** XSS) occurs when an application accepts user input, stores it, and later includes it in HTTP responses. If the application fails to sanitize the input sufficiently, attackers can exploit this vulnerability by including malicious scripts in any submitted data.

When the application generates responses using the stored – unsanitised – input, the malicious scripts are injected into the response and are executed when the page is loaded.
![An example of stored XSS in a web application. The web application gets the name field from the user's name, stored in a database. The payload sent to the application is <script>alert("XSS")</script>. When the page is rendered, an alert box with "XSS" displayed is shown.](https://il-labforge-assets.origin.immersivelabs.team/uploads/WpvASmIwlWC9otB1JaQRGzNNSU4od8b-VgEbftrbF9k.png)

Malicious payloads can persist within a web application for as long as the stored data is used in subsequent HTTP responses, which often makes XSS attacks capable of being triggered repeatedly. 

***Web applications commonly take user input from one part of the application and use this data in other areas, potentially injecting an XSS payload into a different page than where it was initially submitted.***

### Exploiting stored XSS
The response from the server includes unsanitised messages taken from user input, making it vulnerable to stored XSS. These messages are saved on the server and displayed whenever a user loads the "general" channel, so an attacker can inject malicious scripts into this page by including them in a message.
![The messages page. The user "mal" is about to send a message to the application containing the XSS payload.](https://il-labforge-assets.origin.immersivelabs.team/uploads/xR3Qky4luk_lUqo7FPRAERf2ZrruDgU1A8wqwpLMUh4.png)

Once submitted, this message would be saved on the server, like any other message. When these messages are included on a page, the script would be injected into the response:
```html
<p>mal [12:15]: <script>alert("xss")</script></p>
```

### AI Breakdown
Sure, here’s a step-by-step breakdown of a Stored Cross-Site Scripting (XSS) attack:
1. **Input Submission**: The attacker submits a malicious script through a vulnerable input field on a web application. This could be a comment field, a post message, a profile field, or any other input that accepts and stores data.
    
2. **Data Storage**: The web application saves the input with the malicious script in its database without properly sanitizing or encoding it.
    
3. **Rendering the Stored Data**: Later, when the web application retrieves the stored data to display it in a page, it includes the malicious script as part of the HTML content of the page.
    
4. **Script Execution**: When a victim visits the page, their browser executes the malicious script along with the rest of the HTML content. Since the script is executed in the context of the victim’s session, it can access sensitive information such as session cookies, tokens, or other sensitive data.
    
5. **Data Extraction**: The malicious script sends the sensitive data it has accessed to the attacker’s server. This could lead to actions like session hijacking, identity theft, or other malicious activities.

To protect against Stored XSS attacks, it’s crucial to sanitize and escape user inputs before storing them in the database, and also when rendering them on a page. Implementing a strong Content Security Policy (CSP) can also help mitigate the risk of XSS attacks.


