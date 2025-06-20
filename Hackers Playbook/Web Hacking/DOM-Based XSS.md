### DOM-based cross-site scripting (XSS)
An application takes user input and uses it to modify the DOM, typically using client-side scripts like JavaScript. Attackers can exploit this vulnerability by including malicious scripts in their input, which is subsequently injected into the application if not sanitised correctly.

***What distinguishes DOM-based XSS from other types of XSS attacks, such as reflected and stored XSS, is that the payload isn't injected into server responses. Instead, the attack manipulates the intended functionality of the web application, causing the client-side script to behave unexpectedly. Depending on the client-side script, the XSS payload could be executed by the script itself or injected into a different area of the application.***

You can identify any client-side scripts used by an application by examining the application's source code. Scripts can be called inline within `<script>` tags, but some may be located in external files and referenced by the application. For example, a script called 'user.js' could be called with the following HTML.
```html
<script src="/js/user.js"></script>
```
Both methods of executing scripts on a web application can be susceptible to DOM-based XSS.

### Document Object Model
Essentially, the Document Object Model (DOM) is an interface that provides a way for scripts, typically using languages like JavaScript, to ***access*** and ***manipulate*** the content of an HTML document.

The DOM represents an HTML document as a tree-like structure. Each HTML element (e.g., `<body>` or `<div>`) in the document becomes a node in this tree, and these nodes are represented as objects in the DOM. These objects have properties that represent the attributes of each HTML element, methods that allow manipulation of the elements, and events that can be used to respond to user actions or other interactions. For example, a DOM might look like this:
![An example of a DOM structure. Document is at the top of the tree, followed by the html element. Elements shown in the DOM include head, body, title, h1, img, div, and p](https://il-labforge-assets.origin.immersivelabs.team/uploads/4f9RF-jh67ns2x6pnwqwUdAo2zjgtiTOGrT2NSH6B7Q.png)

By using scripting languages like JavaScript, developers can interact with the DOM and manipulate the HTML elements on a webpage. This includes creating new elements, modifying existing elements, changing their attributes or content, and removing elements from the DOM. These changes directly affect the web page's content and structure, allowing dynamic web pages to be created.

### Exploiting DOM-based XSS
#### Using `<script>` tags
Consider an application that can encode messages in a browser:
![An example of a web application where a message is taken from the URL parameter and encoded into base64. Both the original message, and the encoded message, is on the page.](https://il-labforge-assets.origin.immersivelabs.team/uploads/OgVB_eqJ7nzr6hKgI0TAqkCBquAZmBz155jJ_RHEiDM.png)

In this example, the application uses a client-side script to include the user's original message on the page:
```javascript
var queryParam = new URLSearchParams(location.search).get('message');
var query = decodeURIComponent(queryParam);
var originalMessage = "<p>Original message: " + query + "</p>";
document.write(originalMessage);
```

The client-side script retrieves the user's query from the URL and inserts it into the DOM without performing any sanitization – this makes the script vulnerable to DOM-based XSS attacks. If an attacker were to search for the payload `<script>alert("xss")</script>`, this payload would be included in the new object added to the DOM, resulting in the following line of HTML:
```html
<p>Original message: <script>alert("xss")</script></p>
```

When the web page loads, the script is processed and executed as if it were part of the original response from the web server. In this case, it would display an alert box in the user's browser, demonstrating a successful DOM-based XSS attack.
![The same data encoding webpage, with an alert box displaying the message "XSS" on screen](https://il-labforge-assets.origin.immersivelabs.team/uploads/4o53zTsQi2BrK9CtpPUuw3v3wIbnYyGP6mktkjBAthw.png)

#### JavaScript event handlers
It's important to look at the script's source code to see how a payload must be constructed. In some situations, using a simple `<script>` payload may not be sufficient to trigger DOM-based XSS. Take the following script, which takes the value of the username parameter from the URL, and uses it to load the user's avatar:
```javascript
var queryParam = new URLSearchParams(location.search).get('username');
var user = decodeURIComponent(queryParam);
var avatar = "<img src='/images/" + user + "'>";
document.write(avatar);
```

If an attacker tries to use the standard XSS payload tags, such as `<script>alert("xss")</script>`, the following HTML would be added to the DOM:
```html
<img src='/images/<script>alert("xss")</script>' >
```

However, this isn't a valid way to execute JavaScript code, and the script won't be executed. An attacker can exploit JavaScript event handlers to run malicious code in these circumstances. One such event handler is `onerror`, which executes JavaScript code if an error occurs while loading an image, such as when the image cannot be found.

So, if an attacker submitted the payload `nobody' onerror='alert("xss")` to the application, it would cause the script to add the following HTML to the DOM:
```html
<img src='/images/nobody' onerror='alert("xss")' >
```

When the web page loads, the server attempts to load the `/images/nobody` resource, but an error occurs since it doesn't exist. This triggers the `onerror` event handler, which executes the injected JavaScript code (`alert("xss")`), displaying an alert box in the browser.

`onerror` is just one event handler that can be used for XSS. Some event handlers will execute code when an element is loaded (`onload`), whereas some require an additional level of user interaction, such as when an element is clicked (`onclick`) or when the user moves the mouse over the element (`onmouseover`).

#### Constructing valid payloads
Incorrectly formatted HTML elements can prevent code from being executed. To successfully exploit a DOM-based XSS vulnerability, an attacker must ensure the client-side script is not generating malformed HTML elements from their XSS payloads and that any objects added to the DOM are appropriately formatted.

Take a look at the following three payloads; only one of these payloads will cause the client-side script to create a valid HTML element that executes a script when the page is loaded, despite looking very similar:
```html
nobody" onerror="alert("Payload One")
nobody' onerror=alert("Payload Two")
nobody' onerror='alert("Payload Three")
```

**Payload One** causes the script to generate the following HTML:
```html
<img src='/images/nobody" onerror="alert("Payload One")'>
```

The HTML element hasn't closed the `src` attribute correctly, as the payload uses the wrong type of quotation mark to close the string. Therefore, the payload is seen as part of an image filename instead of code to be executed. So, nothing will happen when this HTML is added to the DOM and injected into the page. The image will still fail to load, but no script will be executed.

**Payload Two** uses the appropriate quotation marks, closing the filename in the `src` attribute with a `'` character. However, there's still an unhandled quotation mark at the end of the tag, so this script will create the following HTML:
```html
<img src='/images/nobody' onerror=alert("Payload Two")'>
```

When this HTML element is added to the DOM and injected into the web page, again, nothing will happen, and no script will be executed.

**Payload Three**, however, handles all quotation marks added by the client-side script correctly, and the HTML element is formatted correctly. Therefore, the following HTML will be generated by the script:
```html
<img src='/images/nobody' onerror='alert("Payload Three")'>
```

This time, when injected into the page, the payload will execute – displaying a dialog box with "Payload Three" – successfully demonstrating DOM-based XSS.