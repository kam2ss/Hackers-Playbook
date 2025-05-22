### Payloads
A payload, in its simplest definition, is what will be executed by Metasploit on a successful exploit. Metasploit has hundreds of types of payloads, each attachable to different exploits and designed for a variety of scenarios.

The payloads available will also depend on the target selected. As explored in **Ep.5 -- Exploits**, you can list the targets available by running the `show targets` command and change it by running the `set target` command. Each target has different payloads available to it depending on the module, so it’s important to set the target before listing the available payloads.

### Using payloads
Running the `show payloads` command will display a list of all compatible payloads available to use with your active exploit module and your current target type.

![Payload options for the module 'struts2_rest_xstream'.](https://il-labforge-assets.origin.immersivelabs.team/uploads/r7I4gRtFudE7ja_dqq_T_BU-JbErHjT5IWhlVWrw25k.png)

### Shells
The most common form of payload in Metasploit is a **shell**, and will simply get a shell on the target system. This is typically the first choice in any attacker’s arsenal, providing the attacker with an entrypoint into the target environment, and the ability to launch further attacks from the shell.

### Reverse vs bind shells
Metasploit shells typically fall under two distinct categories, and it’s important to understand the difference between them. The first, **reverse shells**, is a type of shell where the connection is initiated **from the target system**, with the attacking host listening for any remote connections. Conversely, **a bind shell** is a type of shell where a listening port is opened on the target system, and the connection is made from the **attacking host**. Fundamentally, the main difference between these shells is the direction in which the traffic travels when initiating communication.

With this in mind, reverse shell payloads are typically **more successful** than their bind counterparts, even when launched against the same vulnerability on the same system. Well-configured networked systems are more likely to block ingress traffic (data moving into the network) by default than egress traffic (data moving out of the network), so it’s generally more likely that a reverse shell is able to communicate out of the network.

You can identify whether a payload uses a reverse or bind shell simply by its name. For example, `windows/x64/meterpreter/bind_tcp` is a bind shell whereas `windows/x64/meterpreter/reverse_tcp` is a reverse shell.

### Payload type
The final element of the Metasploit payload name that you need to understand is the payload type itself. In all the aforementioned examples, the payload has ended with `bind_tcp` or `reverse_tcp`. This tells you that the payload is a **shell** (either bind or reverse) and that it uses **TCP** to communicate.

If the payload is not a shell, the final element of the payload name will typically be a short descriptor of what the payload does. For example, the `php/exec` payload uses PHP to execute a command on the target system.

![Reverse_tcp payload options.](https://il-labforge-assets.origin.immersivelabs.team/uploads/5tIDb9_Yc3CPlb4g3hkgnJuHyJMHLamxTwdzSnZSTeE.png)
