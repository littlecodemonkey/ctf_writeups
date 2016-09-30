# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

## Level 19
**Description**
To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used to setuid binary.

**Solution**
With an ll it looks like there is a file that can run as bandit20. I ran the file and it told me what commands to use. I tried a whoami to find out what user it ran as, and then proceeded to perform a cat on the bandit password file.

```bash
bandit19@melinda:~$ ll
total 28
drwxr-xr-x   2 root     root     4096 Nov 14  2014 ./
drwxr-xr-x 172 root     root     4096 Jul 10 14:12 ../
-rw-r--r--   1 root     root      220 Apr  9  2014 .bash_logout
-rw-r--r--   1 root     root     3637 Apr  9  2014 .bashrc
-rw-r--r--   1 root     root      675 Apr  9  2014 .profile
-rwsr-x---   1 bandit20 bandit19 7370 Nov 14  2014 bandit20-do*
bandit19@melinda:~$ ./bandit20-do
Run a command as another user.
  Example: ./bandit20-do id
bandit19@melinda:~$ ./bandit20-do whoami
bandit20
bandit19@melinda:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
<password>
```
