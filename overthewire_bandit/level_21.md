# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

## Level 21
**Description**
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

**Solution**
I looked in the cron directory and found a file called bandit22. After looking at this file, I saw it ran a script in usr/bin. Viewing the script I saw that it copied the password to a file in the tmp directory. Viewing this file, I found the next password.

```bash
bandit21@melinda:~$ cd /etc/cron.d
bandit21@melinda:/etc/cron.d$ ls
behemoth4_cleanup  cronjob_bandit23       leviathan5_cleanup    natas-session-toucher  natas25_cleanup~  php5        semtex0-ppc  vortex0
cron-apt           cronjob_bandit24       manpage3_resetpw_job  natas-stats            natas26_cleanup   semtex0-32  semtex5      vortex20
cronjob_bandit22   cronjob_bandit24_root  melinda-stats         natas25_cleanup        natas27_cleanup   semtex0-64  sysstat
bandit21@melinda:/etc/cron.d$ cat cronjob_bandit22
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
bandit21@melinda:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@melinda:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
<password>
```
