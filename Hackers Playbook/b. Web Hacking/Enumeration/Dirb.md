### Fuzzing
Fuzz testing (fuzzing) is a technique in which data is automatically provided as input to an application.

Running a fuzzing tool such as Dirb can help discover files and directories in a web application.
```
dirb http://web-application
```

Dirb can also be extended with custom wordlists by providing the location of a file to `dirb`.
```
dirb http://web-application /usr/share/wordlists/dirb/vulns/jersey.txt
```