# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

### Level 1
**Description:**
The password for the next level is stored in a file called - located in the home directory

**Solution:**
I tried to cat the file at first, but then found that I needed to pass it the path as that didn't work.

```bash
 bandit1@melinda:~$ ls
 -
 bandit1@melinda:~$ cat ./- 
 <password>
```

**Notes:**
* What I've learned
  * I hadn't ran accross a filename of just a hyphen before. You have to pass it a relative or full path for linux to know it's a filename. This makes sense as the hyphen is used quite a bit in parameters in the cat command. For example, -n for numbered lines.
