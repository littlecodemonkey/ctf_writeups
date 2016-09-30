# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup


### Level 13
**Description**
The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on.


**Solution**
First I logged into the box, and there was the private key.
```bash
bandit13@melinda:~$ ls
sshkey.private
```

I just did an scp secure copy over to my host, and then used it to log in. I needed to change the permissions to 600 in order for it to work.
```bash
> scp bandit13@bandit.labs.overthewire.org:~/sshkey.private .
> chmod 600 ./sshkey.private 
> ssh -i ./sshkey.private bandit14@bandit.labs.overthewire.org
```

The password for bandit 14 is stored in /etc/bandit_pass/bandit14
```data
bandit14@melinda:~$ cat /etc/bandit_pass/bandit14
<password>
```
