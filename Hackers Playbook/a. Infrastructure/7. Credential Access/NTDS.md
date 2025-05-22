On a standard Windows installation, ***user accounts are stored in the registry***. For domain controllers, the active directory accounts are stored in a database-style file called `NTDS.dit`.

Online tools are: [esedbexport](https://github.com/libyal/libesedb/blob/main/esedbtools/esedbexport.c), [ntdsxtract](https://github.com/csababarta/ntdsxtract), and [Impacket](https://github.com/fortra/impacket).

#### NTDS.dit
The `ntds.dit` file is a windows Extensible Storage Engine (ESE) database that ***stores sensitive information, including password hashes for every account, computers and security groups in the domain***. However, this file is typically ***locked by the operating system***. 

#### Extracting NTDS.dit
There are two primary methods used to acquire the `ntds.dit` file:
- **Live extraction**: This involves ***dumping the file while the system is running***, typically requires admin or system-level privileges.
- **Offline extraction**: This method ***uses backups or physical access to the DC***, allowing an attacker to retrieve the file without triggering system alerts.

#### Parsing and analysing NTDS.dit
Once you've acquired the `ntds.dit` file, you'll need a couple to tools to parse and analyze the data contained within it.

- **esedbexport**: parses and analyze by ***exporting the contents into tables***. 
	```
	esedbexport -m tables ntds.dit
	```

- **ntdsxtract**: works alongside the data exported by esedbexport. It allows you to query specific data from the `ntds.dit` file, such as user accounts, deleted objects, or domain group information.
	- Key tools with the `ntdsxtract` suite include:
		- **dsusers.py**: extracts individual user account information
		- **dsgroups.py**: displays AD group information
		- **dscomputers.py**: retrieves information about workstations connected to the DC.
	```
	dsusers.py datatable.4 link_table.7 output --syshive ~/pentest/registry/SYSTEM --passwordhashes --pwdformat ocl --ntoutfile ntout --lmoutfile lmout > output.txt
	```
	- `--syshive <path to system hive>`: this flag is necessary for password hash extraction as it helps decrypt the password hashes stored in `ntds.dit`.
	- ```grep '@keyword' output.txt```: to search for the specific strings.

- **Impacket**: a set of Python libraries and example scripts that assist with network protocols. `secretsdump.py` can extract and dump password hashes from a local `ntds.dit` or even directly from a DC.
	```
	secretsdump.py -ntds <ntds.dit path> -system <SYSTEM hive path> LOCAL
	```
