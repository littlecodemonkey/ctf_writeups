# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup
  
### Level 2
**Description:**
The password for the next level is stored in a file called spaces in this filename located in the home directory.

**Solution:**
I was lasy and hit the < tab > key. This let autocomplete file out the filename and escape the spaces for me.

```bash
 bandit2@melinda:~$ ls
 spaces in this filename
 bandit2@melinda:~$ cat spaces\ in\ this\ filename 
 <password>
```
