# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

### Level 8 
**Description**
The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

**Solution**
I sorted this file and checked for uniqe lines with the sort and uniq commands.

```bash
bandit8@melinda:~$ cat ./data.txt | sort | uniq -u
<password>
```
