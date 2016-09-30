# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

### Level 7
**Description**
The password for the next level is stored in the file data.txt next to the word millionth

**Solution**
I just ran a grep command to look for the word "millionth".

```bash
bandit7@melinda:~$ grep "millionth" ./data.txt 
millionth	<password>
```
