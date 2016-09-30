# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

### Level 9 
**Description**
The password for the next level is stored in the file data.txt in one of the few human-readable strings, beginning with several ‘=’ characters.

**Solution**
Grep doesn't work very well because the equals sign keeps on trying to compare if the files are binary equvilant. Something that would be better for a diff command. Before trying to dig too deep into it, I tried a regex in perl. This worked. 

```bash
bandit9@melinda:~$ perl -ne 'print $_ if /\=\=/m' ./data.txt
<password>
```
