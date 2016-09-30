# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

## Level 17
**Description**
There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19

**Solution**
This one seemed pretty easy. I'm not sure I went about it they way they meant for me to. I just ran a diff on the files.

```bash
bandit17@melinda:~$ diff passwords.old passwords.new 
42c42
< BS8bqB1kqkinKJjuxL6k072Qq9NRwQpR
---
> <password>
```