### What is Target?
The Target tool in Burp Suite provides an ***overview of a website's structure, including domains, directories, and files***. It features a ***site map for navigation*** and a ***target scope tool*** to define in-scope and out-of-scope hosts and URLs.

### Site Map - Attack Surface & Hidden Content
The ***Site Map*** tab in Burp ***compiles all gathered information*** about a target application. It allows ***filtering*** and ***annotation*** to aid testing. 

Once the application's visible content is fully mapped, the ***attack surface can be analysed*** using the site map tab. The ***site map helps identify exposed attack vectors and hidden content*** that isn't accessible through standard browsing.

### Target Scope
The ***target scope*** configuration in Burp defines which hosts and URLs are included in your testing.
- Add a URL to the scope by **right-clicking** in the **Site Map** or **HTTP history** and selecting **"Add to Scope"**, or manually via the **Scope tab** under **Target**.
    
- For **advanced scopes**, enable **"Use Advanced Scope Control"** to filter by protocol, port, or regex.

**Include `example.com` and its subdomains**
```
.*\.?example\.com$
```

