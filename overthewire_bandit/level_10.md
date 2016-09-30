# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

### Level 10
**Description**
The password for the next level is stored in the file data.txt, which contains base64 encoded data


**Solution**
The solution to this was to just base64 decode the file.

```bash
bandit10@melinda:~$ base64 -d ./data.txt
The password is <password>
```
