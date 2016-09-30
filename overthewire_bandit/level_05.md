# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

### Level 5
**Description**
The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties: - human-readable - 1033 bytes in size - not executable

**Solution**
Since the file is 1033 bytes in size, I ran a du command and used grep to filter down to files that were this size. There was only one file, so I ran a cat command on that, and it had the next password.

```bash
bandit5@melinda:~$ cd ./inhere/
bandit5@melinda:~/inhere$ du -ab | grep 1033
1033	./maybehere07/.file2
bandit5@melinda:~/inhere$ cat ./maybehere07/.file2 
<password>
```
