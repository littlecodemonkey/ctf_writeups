# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

### Level 3
**Description:**
The password for the next level is stored in a hidden file in the inhere directory.

**Solution**
I found a directory in here so I just changed into the directory and performed an ls to see what was in there.
```bash
bandit3@melinda:~$ ls
inhere
bandit3@melinda:~$ cd inhere/
bandit3@melinda:~/inhere$ ls
```

After not seeing anything I tried an ls -al to check for hidden files. Files that start with a period are hidden in linux and this is pretty common in filesystems. It was there so I just ran a cat to view the contents, and I'm quickly on my way to the next flag.
```bash
bandit3@melinda:~/inhere$ ls -al
total 12
drwxr-xr-x 2 root    root    4096 Nov 14  2014 .
drwxr-xr-x 3 root    root    4096 Nov 14  2014 ..
-rw-r----- 1 bandit4 bandit3   33 Nov 14  2014 .hidden
bandit3@melinda:~/inhere$ cat .hidden 
<password>
```

**TL;DR**
```bash
bandit3@melinda:~$ ls
inhere
bandit3@melinda:~$ cd inhere/
bandit3@melinda:~/inhere$ ls
bandit3@melinda:~/inhere$ ls -al
total 12
drwxr-xr-x 2 root    root    4096 Nov 14  2014 .
drwxr-xr-x 3 root    root    4096 Nov 14  2014 ..
-rw-r----- 1 bandit4 bandit3   33 Nov 14  2014 .hidden
bandit3@melinda:~/inhere$ cat .hidden 
<password>
```

