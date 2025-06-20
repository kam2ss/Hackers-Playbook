### Fuzzing
Fuzz testing (fuzzing) is a technique in which ***data is automatically provided as input to an application***.

Another fuzzing tool is Wfuzz, similar to Dirb and can be used to ***brute force files and directories*** on a target application if supplied with a wordlist.

```
wfuzz -z file,/usr/share/wordlists/dirb/common.txt http://webapplication/FUZZ
```

Wfuzz also supports response filtering. For example, if provided with the `--sc` flag and the response code you wish to filter (e.g. `--sc=200`), Wfuzz will only show responses that match that response code.
```
wfuzz -z file,/usr/share/wordlists/dirb/common.txt --sc=200 http://webapplication/FUZZ
```

Wfuzz expands on Dirb by introducing advanced fuzzing techniques, including fuzzing ***cookies***, ***server headers***, and even ***HTTP methods***.

Sometimes, ***developers may leave HTTP methods enabled*** while debugging or testing their applications.

The following command uses Wfuzz to ***request the domain using each of the HTTP methods*** specified:
```
wfuzz -z list,GET-HEAD-POST-TRACE-OPTIONS -X FUZZ http://web-application
```

`Note: FUZZ word keyword is a placeholder that tells Wfuzz where to insert the words from the specified wordlists`.
