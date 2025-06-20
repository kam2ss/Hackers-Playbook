To capture HTTPS traffic, the browser must use Burp as its proxy for both protocols. Since ***Burp breaks the TLS connection***, security warnings appear, but they can be ***bypassed by adding an exception***. ***HSTS enforces HTTPS and blocks invalid certificates***, but ***adding Burp's CA certificate as a trusted authority*** in Firefox allows interception without warnings.

**To install Burp's CA certificate:**
1. **Download the Certificate**
    - With Burp running, visit **[http://burp](http://burp)** and download the **CA Certificate**.
    
2. **Install in Firefox**
    - Open **Firefox Settings** → **Privacy & Security**.
    - Click **View Certificates** → **Authorities** → **Import**.
    - Select the downloaded **Burp CA** file and check the option to trust it for identifying websites.

3. **Install in Chrome**
    - Chrome uses the system’s certificate store.
    - Install the **Burp CA** in your OS's built-in browser (e.g., Internet Explorer on Windows, Safari on macOS).
    - Chrome will automatically trust it.
    - You can manage certificates in **Chrome Settings → Privacy & Security → HTTPS/SSL**.