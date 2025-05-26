NFS (Network File Sharing) is a distributed file system protocol, allowing a user on a client computer to access files over a computer network much like local storate is accessed.
	cat /etc/exports ![[Pasted image 20230704205726.png]]
	
The critical element for this privilege escalation vector is the "no_root_squash" option.  If the “no_root_squash” option is present on a writable share, we can create an executable with SUID bit set and run it on the target system.  

From Attacker Machine:
We will start by enumerating mountable shares from our "attacking machine".
![[Pasted image 20230704210318.png]]

We will mount one of the “no_root_squash” shares to our attacking machine and start building our executable.
![[Pasted image 20230704210426.png]]

As we can set SUID bits, a simple executable that will run /bin/bash on the target system will do the job. So create a file under newly created folder:
	int main()
	{  setgid(0);
	  setuid(0);
	  system("/bin/bash");
	  return 0;
	}

Once we compile the code we will set the SUID bit.
![[Pasted image 20230704211755.png]]

On the target machine, check both "nfs.c and nfs" are present under "/backups" folder
![[Pasted image 20230704220234.png]]