# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

### Level 0
**Description:**
The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.

**Solution:**
First I ssh'd into the server.
```bash
 > ssh bandit0@bandit.labs.overthewire.org
```

Next it asked for the password and I entered bandit0, as that was the one in the description. Instictively, I ran a ls command to check the contents from the directory and there was a readme file.
```bash
 bandit0@melinda:~$ ls
 readme
```

I thought I would run a cat command to view the contents of the file, there were a string of characters in the file. That's the next password.
```bash
 bandit0@melinda:~$ cat readme
 <password>
```

**TL;DR**
```bash
 > ssh bandit0@bandit.labs.overthewire.org
 bandit0@melinda:~$ ls
 readme
 bandit0@melinda:~$ cat readme
 <password>
```