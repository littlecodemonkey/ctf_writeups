# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

### Level 6
**Description**
The password for the next level is stored somewhere on the server and has all of the following properties: - owned by user bandit7 - owned by group bandit6 - 33 bytes in size

**Solution**
The du command is not going to work as well for this because there will be a lot of 33 byte files on the server. Since we know the user and group I went with the find comnand instead. Also, since I'm bound to get a lot of permission denied errors, I sent those into /dev/null to get rid of them.

```bash
bandit6@melinda:~$ find / -size 33c -user bandit7 -group bandit6  2>/dev/null/var/lib/dpkg/info/bandit7.password
bandit6@melinda:~$ cat /var/lib/dpkg/info/bandit7.password
<password>
```
