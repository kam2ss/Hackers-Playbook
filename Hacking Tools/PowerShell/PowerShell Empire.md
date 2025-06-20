### Overview
PowerShell Empire is a popular post-exploitation agent written for PowerShell. It provides the ability to create deployable PowerShell implants with its own Command and Control (C2) server, using cryptologically secure communication. PowerShell Empire allows adversaries to have full .NET access, application allowlisting and direct access to the Win32 API. Additionally, PowerShell Empire features a range of offensive modules, such as privilege escalation, Mimikatz integration, persistence and lateral movement.

### Episode 1
The purpose of this lab is to give an introduction to the basics of the tool. We will therefore be covering Listeners and Stagers.

#### Listeners
A listener is the beacon that is used to listen for incoming connections from our soon-to-be infected hosts. Once Empire has been run, the listeners panel can be accessed by typing `listeners` into the console.

A listener must then be selected; to do this, type `uselistener`. We will be using the HTTP listener, so the final command will be `uselistener http`.

With the HTTP listener selected, we can now type `info` to see a list of available configurations. These can be easily changed in a manner similar to how listeners were selected.

For example, to change the setting to make our HTTP listener listen from our server's IP address, we just need to type:

`set Host http://<our_server_ip>:<port_number>`

You can also set the Port setting with:

`set Port <port_number>`

Once the configuration is finished, to activate the listener, we can type:

`execute`

#### Stagers
Stagers are essentially the initial payload used during post-exploitation. They pull down the main agent/implant that is used to gain control over a machine. In the wild, stagers will often be hidden in word macros in malicious documents. When the document is opened and the stager is launched, it will call back to the listener, which will then send the agent to the infected host. Having a stager separate from the primary payload is important as it helps evade antivirus software.

It's important to back out to the main menu using the `back` command before trying to access the stager configuration.

To configure a stager inside PowerShell Empire, we can type `usestager` and tab complete and view all the different types of stagers that can be used. Once a stager is selected, it can be configured similarly to a listener. Typing `info` will reveal the different configuration settings. To link the stager to a listener, the following command can be used: `set Listener http`. To generate the stager, we need to type `generate`. This will do all the hard work for us and create a ready-to-use stager that can now be sent and used.