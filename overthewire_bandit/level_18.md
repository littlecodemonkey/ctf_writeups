# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

## Level 18
**Description**
The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

**Solution**
Once again, there's probably another way you're supposed to do this, but I just grabbed the file with secure copy.

```bash
> scp bandit18@bandit.labs.overthewire.org/~/readme .
> cat ./readme
<password>
```