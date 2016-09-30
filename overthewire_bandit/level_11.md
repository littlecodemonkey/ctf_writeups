# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

### Level 11
**Description**
The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

**Solution**
This is a ROT13 cipher which I just used another regex, this on a tr, to get by.
```bash
bandit11@melinda:~$  perl -pe 'tr/A-Za-z/N-ZA-Mn-za-m/' ./data.txt
The password is <password>
```
