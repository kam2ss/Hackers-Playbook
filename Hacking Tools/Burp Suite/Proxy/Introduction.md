### What is Burp Suite?
An ***application proxy*** that can be configured to route the traffic through Burp Suite, and thus traffic can be captured and analysed.

### What is Web Proxy?
A web proxy is an intermediary server that handles website traffic between clients and web servers. It provides a layer of ***security by masking a user's real IP address*** or allowing organizations to ***filter the web content*** that is accessed on their network. It can also ***bypass geo-blocking***, and can ***improve browsing speed through caching***.

Tools like Burp Suite use web proxies to capture and manipulate HTTP traffic for security testing.

### Burp Suite: Proxy
The Proxy tab is the core of the Burp Suite framework, and it contains four sub-tasks:

**Intercept**
Burp Proxy ***captures and holds HTTP requests*** until they are forwarded or dropped. Users can edit requests before forwarding them to test application behaviour.

**HTTP History**

**WebSockets History**

**Proxy Settings**