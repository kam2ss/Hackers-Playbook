Banner grabbing is the process of collecting banners from remote servers and devices on a network. Many tools can be used for banner grabbing, including Netcat, Telnet, Curl and Nmap.

Services will often respond automatically to new connections with the service banner. In the case of HTTP, servers are sometimes configured to only respond if a specific request is made. When attempting to banner grab with an HTTP server, we can type `HTTP/1.1 200` after opening the connection to make this request. Sometimes it is necessary to hit enter a few times for the server to respond.

### Banner grabbing with Netcat/Telnet
`Telnet` and `Netcat` are common networking utility tools used to interact with network connections using TCP and UDP. We can use Netcat or Telnet to open a connection to the remote services which respond with their banners. Usage examples of both the Telnet and Netcat commands can be found below:
```bash
# Grab the FTP banner using netcat
nc -v $ipaddress 21

# Grab the SSH banner using telnet
telnet $ipaddress 22
```

### Banner grabbing with Curl
`Curl` is a tool for transferring data to and from network servers. We can use it with the `-I` option to show the header of the requested page. We can then use grep to find the service header or banner. Here's an example of a curl command that will grab the HTTP headers and a grep command that will grab the service banner:
```bash
curl -I $ipaddress | grep -e "Server:"
```

### Banner grabbing with Nmap
`Nmap` has an Nmap Scripting Engine (NSE) script for banner grabbing, which makes a connection to open TCP ports for a specified target and then prints out anything sent by the listening service within five seconds. Nmap will return all of the headers for listening services with the following command:
```bash
nmap -sV --script=banner $ipaddress
```