# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

## Level 22
**Description**
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

**Solution**
From the previous level, I remember a cronjob_bandit23 in the ect/cron.d folder so I cat the file. It shows me the cronjob shell script that runs. Reading the shell script I can tell that the password is copied into a file in the tmp directory. This filename is the md5sum of the text "I am user bandit23". After finding that out, it's as simple as viewing the file, and there I found the password for bandit 23.

```bash
bandit22@melinda:/etc/cron.d$ cat cronjob_bandit23
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@melinda:/etc/cron.d$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
bandit22@melinda:/etc/cron.d$ echo I am user bandit23 | md5sum
8ca319486bfbbc3663ea0fbe81326349  -
bandit22@melinda:/etc/cron.d$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
<password>
```