You can use the `tr` command to manipulate text without needing to make multiple changes manually – think of the *find and replace* feature in a Word document. `tr` takes input and replaces all instances of a specific string with another, with the command `tr [original_string] [string_to_replace]`.

If you entered the command `tr [a] [b]` in your terminal, it would look like nothing is happening. This is because `tr` waits for input from the terminal to perform its replacement by default. If we wanted to make `tr` act on a file, we would need to use `redirection` operators: the less than (<) and greater than (>) characters.

By adding `< file` to our original `tr [a] [b]` command, `tr` would perform any replacement on the file's content rather than wait for input from the command line. You can also add `> filename` to the command if you’d rather the output be saved directly to a file than output the results to the terminal.

If we put all that together, the command `tr [a] [b] < myfile.txt > newfile.txt` would:
- Take the contents of the file `myfile.txt`
- Replace every letter `a` with a letter `b`
- Save the results to a file called ‘newfile.txt’. If this file exists, overwrite it.
  
Instead of writing `tr [abcd...xyz] [ABCD...XYZ]`, you can use the sets [:lower:] and [:upper:] in the command: `tr [:lower:] [:upper:] < lowercase`.

Here are some other useful character groupings which `tr` is aware of:
`[:alnum:]` All letters and digits (A–Z, a–z, 0–9)  
`[:alpha:]` All letters (A–Z, a–z)  
`[:digit:]` All digits (0–9)  
`[:punct:]` All punctuation characters

You can also use `tr` to remove character sets from a file. Use the command `tr -d [:chars:]` to **remove** a character set.


Examples:
changing letter *lower* to *upper*:
```
tr [:lower:] [:upper:] < filename > newfile
```

Removing *punctuation*:
```
tr -d [:punct:] < filename > newfile
```

Replacing '{}' with '[]':
```
tr '{}' '[]' > filename > newfile
```

