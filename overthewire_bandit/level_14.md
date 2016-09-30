# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

### Level 14
**Description**
The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

**Solution**
I did this with a netcat.

```data
bandit14@melinda:~$ echo "<banditl4_password>" | nc localhost 30000
Correct!
<bandit15_password>

```
