# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

## Level 25
**Description**
Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.


**Solution**
Logging in there is an ssh key to the next level. So I grabbed that to my own box and chmod 600 the permissions, and tried to log in. It immediatly logged me out. (Worth a shot)

```bash
bandit25@melinda:~$ ls
bandit26.sshkey
> scp bandit25@bandit.labs.overthewire.org:~/bandit26.sshkey .
> chmod 600 bandit26.sshkey
> ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org
```

Since the hint was the user is not /bin/bash I tried to figure out what it was by viewing the passwd file. It was using some script called showtext.
```bash
bandit25@melinda:~$ cat /etc/passwd |grep bandit
bandit0:x:11000:11000:bandit level 0:/home/bandit0:/bin/bash
bandit1:x:11001:11001:bandit level 1:/home/bandit1:/bin/bash
bandit2:x:11002:11002:bandit level 2:/home/bandit2:/bin/bash
bandit3:x:11003:11003:bandit level 3:/home/bandit3:/bin/bash
bandit4:x:11004:11004:bandit level 4:/home/bandit4:/bin/bash
bandit5:x:11005:11005:bandit level 5:/home/bandit5:/bin/bash
bandit6:x:11006:11006:bandit level 6:/home/bandit6:/bin/bash
bandit7:x:11007:11007:bandit level 7:/home/bandit7:/bin/bash
bandit8:x:11008:11008:bandit level 8:/home/bandit8:/bin/bash
bandit9:x:11009:11009:bandit level 9:/home/bandit9:/bin/bash
bandit10:x:11010:11010:bandit level 10:/home/bandit10:/bin/bash
bandit11:x:11011:11011:bandit level 11:/home/bandit11:/bin/bash
bandit12:x:11012:11012:bandit level 12:/home/bandit12:/bin/bash
bandit13:x:11013:11013:bandit level 13:/home/bandit13:/bin/bash
bandit14:x:11014:11014:bandit level 14:/home/bandit14:/bin/bash
bandit15:x:11015:11015:bandit level 15:/home/bandit15:/bin/bash
bandit17:x:11017:11017:bandit level 17:/home/bandit17:/bin/bash
bandit18:x:11018:11018:bandit level 18:/home/bandit18:/bin/bash
bandit19:x:11019:11019:bandit level 19:/home/bandit19:/bin/bash
bandit20:x:11020:11020:bandit level 20:/home/bandit20:/bin/bash
bandit21:x:11021:11021:bandit level 21:/home/bandit21:/bin/bash
bandit22:x:11022:11022:bandit level 22:/home/bandit22:/bin/bash
bandit23:x:11023:11023:bandit level 23:/home/bandit23:/bin/bash
bandit25:x:11025:11025:bandit level 25:/home/bandit25:/bin/bash
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
bandit24:x:11024:11024:bandit level 24:/home/bandit24:/bin/bash
bandit16:x:11016:11016:bandit level 16:/home/bandit16:/bin/bash
```

Viewing the showtext file, I see it runs a more on this text.txt file and then exits. I looked to see if I could edit this file, but root owns it. I then looked to see if I could edit the text.txt file, but no luck.
```bash
bandit25@melinda:~$ cat /usr/bin/showtext 
#!/bin/sh

more ~/text.txt
exit 0

bandit25@melinda:~$ ls -l /usr/bin/showtext 
-rwxr-xr-x 1 root root 34 Nov 16  2014 /usr/bin/showtext

bandit25@melinda:~$ ls -al ../bandit26/
total 32
drwxr-xr-x   3 root     root     4096 Nov 16  2014 .
drwxr-xr-x 172 root     root     4096 Jul 10 14:12 ..
-rw-r--r--   1 root     root      220 Apr  9  2014 .bash_logout
-rw-r--r--   1 root     root     3637 Apr  9  2014 .bashrc
-rw-r--r--   1 root     root      675 Apr  9  2014 .profile
drwxr-xr-x   2 root     root     4096 Nov 16  2014 .ssh
-rw-------   1 bandit26 bandit26  430 Nov 16  2014 README.txt
-rw-r-----   1 bandit26 bandit26  258 Nov 16  2014 text.txt

```

It's also so small that the more command wouldn't be needed, so maybe there was something I could use there. I wish I could say I knew the answer right away, but I had to read the man for more, and you can change to vi in visual move by pressing the 'v' button. At this point, I'm able to run other vi commands like... :r /etc/bandit_pass/bandit26 to read the password.

```bash
> ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org
> v                     _ _ _   ___   __
 | |                   | (_) | |__ \ / /
 | |__   __ _ _ __   __| |_| |_   ) / /_
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/
> :r /etc/bandit_pass/bandit26
<password>
```