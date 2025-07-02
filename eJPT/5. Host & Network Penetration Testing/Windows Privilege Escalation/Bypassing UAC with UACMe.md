User Account Control (UAC) is a mechanism for restricting access and allowing accounts to run applications as other users or administrators.

When Windows shows the UAC prompt, it creates a virtual desktop known as the `secure desktop`. This desktop has strict permissions in place, as it runs with system privileges, and that is where the vulnerability begins.

### CVE-2019-1388 Exploit
As with this vulnerability, there is an object ID that existed on older signed executables that would render a ***clickable hyperlink within the certificate information page*** (as can be seen in the images below).

When clicked, the ***UAC starts a browser process and attempts to navigate to the link***. This browser is not started within the context of the secure desktop, so when you dismiss all the dialogues you are presented with an application running as system that can then be used to access and start other applications.

The process to gain system privileges from a standard user account is fairly simple, and it runs in two phases:
- Start the browser as a system user
- Launch cmd.exe as a system user

**Open a browser window as system user**
1. Transfer a signed .exe file to the target system
2. Right click the .exe file and run as administrator
3. Click ‘show more details’
4. Click ‘show information about the publisher’s certificate'
5. Click the ‘issued by’ hyperlink
6. Click ‘OK’
7. Click ‘no’ on the UAC window

**Launch cmd.exe as a system user**
1. Once the page fails to load, click the cog icon in the top right
2. Select File → Save As
3. Accept the warning message by clicking ‘OK’
4. In File, name type c:\windows\system32\*.*
5. Locate cmd.exe and right-click → Open