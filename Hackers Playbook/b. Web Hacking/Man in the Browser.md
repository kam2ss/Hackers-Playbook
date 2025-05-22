### Man-in-the-browser attacks
Man-in-the-browser (MITB) attacks ***use malware or malicious browser extensions to intercept and alter web traffic*** between a user and their browser. Unlike traditional MITM attacks, MITB ***bypasses protections like TLS and multi-factor authentication***. These attacks are commonly used to manipulate online banking transactions by changing payment details before submission, redirecting funds to the attacker.

### Prevention
`Out-of-band authentication:` It uses two different security channels to verify a user's identity (e.g. SMS, mobile app)

### Browser Extension
It consists of:
- `manifest.json:` Defines the extension's metadata, required files, and permissions (e.g., access to open tabs and all HTTP domains.)
- `background.js:` Runs in the background (persistently if set) and stores user input data across sessions.
- `content.js:` Injected into webpages to manipulate the DOM, capturing and modifying user inputs, which are sent to background.js.

To execute a successful MITB attack, the extension must modify a transaction to steal £500, conceal this from the user, and confirm it using `verify-extension` in the extension directory.

**Important**
To main the transaction integrity so the victim doesn't know that something dangerous is going on in the background. 
- Element inspection to grab some span IDs.
- In order to get the right IDs, we'll need to see what the victim is seeing during a transfer.