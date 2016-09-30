# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

## Level 20
**Description**
There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

NOTE: To beat this level, you need to login twice: once to run the setuid command, and once to start a network daemon to which the setuid will connect.

NOTE 2: Try connecting to your own network daemon to see if it works as you think

**Solution**
I had to open two connections. One to listen and one to submit. Luckilly, the description gave me all of the information i needed to know. I picked port 12345. The order was... 
1. run a netcat listen on terminal2. 
2. run the suconnect on terminal1 
3. Send the password over terminal2.

```bash
---Terminal1
bandit20@melinda:~$ ./suconnect
Usage: ./suconnect <portnumber>
This program will connect to the given port on localhost using TCP. If it receives the correct password from the other side, the next password is transmitted back.

bandit20@melinda:~$ ./suconnect 12345
Read: <bandit20_password>
Password matches, sending next password


--- Terminal2
bandit19@melinda:~$ netcat -l 12345
<bandit20_password>
<bandit21_password>
```