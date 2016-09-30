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

  
### Level 2
**Description:**
The password for the next level is stored in a file called spaces in this filename located in the home directory.

**Solution:**
I was lasy and hit the <tab> key. This let autocomplete file out the filename and escape the spaces for me.

```bash
 bandit2@melinda:~$ ls
 spaces in this filename
 bandit2@melinda:~$ cat spaces\ in\ this\ filename 
 <password>
```

### Level 3
**Description:**
The password for the next level is stored in a hidden file in the inhere directory.

**Solution**
I found a directory in here so I just changed into the directory and performed an ls to see what was in there.
```bash
bandit3@melinda:~$ ls
inhere
bandit3@melinda:~$ cd inhere/
bandit3@melinda:~/inhere$ ls
```

After not seeing anything I tried an ls -al to check for hidden files. Files that start with a period are hidden in linux and this is pretty common in filesystems. It was there so I just ran a cat to view the contents, and I'm quickly on my way to the next flag.
```bash
bandit3@melinda:~/inhere$ ls -al
total 12
drwxr-xr-x 2 root    root    4096 Nov 14  2014 .
drwxr-xr-x 3 root    root    4096 Nov 14  2014 ..
-rw-r----- 1 bandit4 bandit3   33 Nov 14  2014 .hidden
bandit3@melinda:~/inhere$ cat .hidden 
<password>
```

**TL;DR**
```bash
bandit3@melinda:~$ ls
inhere
bandit3@melinda:~$ cd inhere/
bandit3@melinda:~/inhere$ ls
bandit3@melinda:~/inhere$ ls -al
total 12
drwxr-xr-x 2 root    root    4096 Nov 14  2014 .
drwxr-xr-x 3 root    root    4096 Nov 14  2014 ..
-rw-r----- 1 bandit4 bandit3   33 Nov 14  2014 .hidden
bandit3@melinda:~/inhere$ cat .hidden 
<password>
```


### Level 4
**Description:**
The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the "reset" command.

**Solution**
Since the file is pretty small, I just ran a recursive grep to look through all of the files for the human readable one.

```bash
bandit4@melinda:~$ cd inhere/
bandit4@melinda:~/inhere$ grep -R .
-file08:??M?????#8B0wPg?????C???@??FM?
-file05:??hZ????P???????{#??TP??6?]??X:
grep: -file07~: Permission denied
-file07:<password>
-file04:????+??5?`???R
-file04:?1*6C?u#Nr?
-file00:;?-i?(??z??????????8???
-file01:??@c
            O8?L??c???7?zb~???????U?
-file06:????!??>P?
-file06:??d{??????H???xX|?
-file03:?:n????8S????[?/q?(??@??M?.?t
#[:*??????j???U?
-file02:?g?f?4?6+>"??B?Vx??d??;de?O
```

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

### Level 6
**Description**
The password for the next level is stored somewhere on the server and has all of the following properties: - owned by user bandit7 - owned by group bandit6 - 33 bytes in size

**Solution**
The du command is not going to work as well for this because there will be a lot of 33 byte files on the server. Since we know the user and group I went with the find comnand instead. Also, since I'm bound to get a lot of permission denied errors, I sent those into /dev/null to get rid of them.

```bash
bandit6@melinda:~$ find / -size 33c -user bandit7 -group bandit6  2>/dev/null/var/lib/dpkg/info/bandit7.password
bandit6@melinda:~$ cat /var/lib/dpkg/info/bandit7.password
<password>
```

### Level 7
**Description**
The password for the next level is stored in the file data.txt next to the word millionth

**Solution**
I just ran a grep command to look for the word "millionth".

```bash
bandit7@melinda:~$ grep "millionth" ./data.txt 
millionth	<password>
```

### Level 8 
**Description**
The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

**Solution**
I sorted this file and checked for uniqe lines with the sort and uniq commands.

```bash
bandit8@melinda:~$ cat ./data.txt | sort | uniq -u
<password>
```

### Level 9 
**Description**
The password for the next level is stored in the file data.txt in one of the few human-readable strings, beginning with several ‘=’ characters.

**Solution**
Grep doesn't work very well because the equals sign keeps on trying to compare if the files are binary equvilant. Something that would be better for a diff command. Before trying to dig too deep into it, I tried a regex in perl. This worked. 

```bash
bandit9@melinda:~$ perl -ne 'print $_ if /\=\=/m' ./data.txt
<password>
```


### Level 10
**Description**
The password for the next level is stored in the file data.txt, which contains base64 encoded data


**Solution**
The solution to this was to just base64 decode the file.

```bash
bandit10@melinda:~$ base64 -d ./data.txt
The password is <password>
```


### Level 11
**Description**
The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

**Solution**
This is a ROT13 cipher which I just used another regex, this on a tr, to get by.
```bash
bandit11@melinda:~$  perl -pe 'tr/A-Za-z/N-ZA-Mn-za-m/' ./data.txt
The password is <password>
```

### Level 12
**Description**
The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

**Solution**
I made a temp directory and copied the file over there, and then did a cat on the file to verify it was a hexdump. Next I ran xxd with the -r parameter to reverse the hexdump into an output file.

```data
bandit12@melinda:~$ mkdir /tmp/tempfolder001
bandit12@melinda:~$ cd /tmp/tempfolder001
bandit12@melinda:/tmp/tempfolder001$ cp ~/data.txt .
bandit12@melinda:/tmp/tempfolder001$ file data.txt
data.txt: ASCII text
bandit12@melinda:/tmp/tempfolder001$ cat data.txt
0000000: 1f8b 0808 34da 6554 0203 6461 7461 322e  ....4.eT..data2.
0000010: 6269 6e00 013f 02c0 fd42 5a68 3931 4159  bin..?...BZh91AY
0000020: 2653 5982 c194 8a00 0019 ffff dbfb adfb  &SY.............
0000030: bbab b7d7 ffea ffcd fff7 bfbf 1feb eff9  ................
0000040: faab 9fbf fef2 fefb bebf ffff b001 3b18  ..............;.
0000050: 6400 001e a000 1a00 6468 0d01 a064 d000  d.......dh...d..
0000060: 0d00 0034 00c9 a320 001a 0000 0d06 80d1  ...4... ........
0000070: a340 01b4 98d2 3d13 ca20 6803 40d1 a340  .@....=.. h.@..@
0000080: 1a00 0340 0d0d 0000 000d 0c80 6803 4d01  ...@........h.M.
0000090: a3d4 d034 07a8 0683 4d0c 4034 069e 91ea  ...4....M.@4....
00000a0: 0f50 1a1a 1ea3 40e9 ea0c 80d0 0346 87a9  .P....@......F..
00000b0: a006 8193 4340 d320 c403 2064 00c4 000c  ....C@. .. d....
00000c0: 8640 0d00 0d06 8340 0c9a 0068 0000 6468  .@.....@...h..dh
00000d0: 1854 0084 0008 38c4 7c28 66b3 bf1f 366d  .T....8.|(f...6m
00000e0: 3971 1c93 f09a 6287 0cfe 04d3 efa9 4164  9q....b.......Ad
00000f0: 0ad1 1828 6c55 75ff 6922 dedd 8cfe 5936  ...(lUu.i"....Y6
0000100: e351 7ae8 0590 6c01 0446 5f2a ba7e 8503  .Qz...l..F_*.~..
0000110: a710 a38c d8c1 9781 5249 b909 8d92 5e09  ........RI....^.
0000120: b343 32a1 9890 cc63 74f2 a3a1 f260 3afa  .C2....ct....`:.
0000130: 4f55 cc30 f7a3 5c20 d610 a588 1ab4 543c  OU.0..\ ......T<
0000140: 71b3 d052 8980 010a b270 4112 89c4 ad7a  q..R.....pA....z
0000150: 8386 125d a460 3a11 3da3 4949 a01f 9e7d  ...].`:.=.II...}
0000160: 8f5e fef5 e13a 4537 dfb3 a898 92e8 cca0  .^...:E7........
0000170: 155c fb29 d0e1 08cf 0cec 7006 b1bc 8f39  .\.)......p....9
0000180: 51bc 1b7b e1ef 161f f020 6830 b1fd d69c  Q..{..... h0....
0000190: e096 54a1 1a03 47ce c4f1 00c7 e520 2e02  ..T...G...... ..
00001a0: 5577 63ac 3dc9 0f84 200a 745d 0503 f8f4  Uwc.=... .t]....
00001b0: b9fb 1152 1c22 a410 572e 11ac cf9e 5ff6  ...R."..W....._.
00001c0: dbf4 ef68 3010 7e36 026e aa38 19fd 4c37  ...h0.~6.n.8..L7
00001d0: 392c a262 f646 8710 9231 4ee4 5200 c601  9,.b.F...1N.R...
00001e0: 529a fec3 8c89 f85d 5f12 5c2f 9073 4544  R......]_.\/.sED
00001f0: 4fed fb97 a851 f831 cd9a 69d7 e80b 12b5  O....Q.1..i.....
0000200: fb37 ba20 86e9 92a7 78c5 5092 2bac 6269  .7. ....x.P.+.bi
0000210: 01c7 09a1 fda4 ef8b 7c14 1832 a30f db92  ........|..2....
0000220: d345 a9b4 de57 8996 4dc7 8ee8 b334 02b2  .E...W..M....4..
0000230: 8dc4 a6a6 08ea c285 d28c 9f60 6779 540a  ...........`gyT.
0000240: 2b97 5e3f f82c 1800 80f1 32b0 32d1 7724  +.^?.,....2.2.w$
0000250: 5385 0908 2c19 48a0 d123 d96f 3f02 0000  S...,.H..#.o?...


bandit12@melinda:/tmp/tempfolder001$ xxd -r ./data.txt > output

```

Then I used the file command to see what type of file it was. I then extracted the file, but it was nested into another file. They really nested this one in there, and I needed to extract several different times before I was at a data file.

```bash
bandit12@melinda:/tmp/tempfolder001$ file output
output: gzip compressed data, was "data2.bin", from Unix, last modified: Fri Nov 14 10:32:20 2014, max compression

bandit12@melinda:/tmp/tempfolder001$ mv output output.gz; gunzip output.gz
bandit12@melinda:/tmp/tempfolder001$ ls
data.txt  output

bandit12@melinda:/tmp/tempfolder001$ file output
output: bzip2 compressed data, block size = 900k

bandit12@melinda:/tmp/tempfolder001$ bunzip2 output
bunzip2: Can't guess original name for output -- using output.out

bandit12@melinda:/tmp/tempfolder001$ file output.out
output.out: gzip compressed data, was "data4.bin", from Unix, last modified: Fri Nov 14 10:32:20 2014, max compression

bandit12@melinda:/tmp/tempfolder001$ mv output.out output.gz; gunzip output.gz
bandit12@melinda:/tmp/tempfolder001$ file output
output: POSIX tar archive (GNU)

bandit12@melinda:/tmp/tempfolder001$ tar -xvf output
data5.bin

bandit12@melinda:/tmp/tempfolder001$ file data5.bin
data5.bin: POSIX tar archive (GNU)

bandit12@melinda:/tmp/tempfolder001$ tar -xvf data5.bin
data6.bin

bandit12@melinda:/tmp/tempfolder001$ file data6.bin
data6.bin: bzip2 compressed data, block size = 900k

bandit12@melinda:/tmp/tempfolder001$ bunzip2 data6.bin
bunzip2: Can't guess original name for data6.bin -- using data6.bin.out

bandit12@melinda:/tmp/tempfolder001$ file ./data6.bin.out
./data6.bin.out: POSIX tar archive (GNU)

bandit12@melinda:/tmp/tempfolder001$ tar -xvf ./data6.bin.out
data8.bin

bandit12@melinda:/tmp/tempfolder001$ file data8.bin
data8.bin: gzip compressed data, was "data9.bin", from Unix, last modified: Fri Nov 14 10:32:20 2014, max compression

bandit12@melinda:/tmp/tempfolder001$ mv data8.bin data8.gz; gunzip data8.gz
bandit12@melinda:/tmp/tempfolder001$ file data8
data8: ASCII text

bandit12@melinda:/tmp/tempfolder001$ cat data8
The password is <password>
```


### Level 13
**Description**
The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on.


**Solution**
First I logged into the box, and there was the private key.
```bash
bandit13@melinda:~$ ls
sshkey.private
```

I just did an scp secure copy over to my host, and then used it to log in. I needed to change the permissions to 600 in order for it to work.
```bash
> scp bandit13@bandit.labs.overthewire.org:~/sshkey.private .
> chmod 600 ./sshkey.private 
> ssh -i ./sshkey.private bandit14@bandit.labs.overthewire.org
```

The password for bandit 14 is stored in /etc/bandit_pass/bandit14
```data
bandit14@melinda:~$ cat /etc/bandit_pass/bandit14
<password>
```

### Level 14
**Description**
The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

**Solution**
I did this with a netcat.

```data
bandit14@melinda:~$ echo "<banditl4_password>" | nc localhost 30000
Correct!
<bandit15_password>

```

### Level 15
**Description**
The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.

Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…


**Solution**
The -ign_eof command was actually really helpfull. I left it off the first time I tried it and I got the heartbeating error. This is the solution I came up with on the second try.

```bash
bandit15@melinda:~$ openssl s_client -connect localhost:30001 -ign_eof
CONNECTED(00000003)
depth=0 CN = li190-250.members.linode.com
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = li190-250.members.linode.com
verify return:1
---
Certificate chain
 0 s:/CN=li190-250.members.linode.com
   i:/CN=li190-250.members.linode.com
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIC3jCCAcagAwIBAgIJAI5QiWZw4YHbMA0GCSqGSIb3DQEBCwUAMCcxJTAjBgNV
BAMTHGxpMTkwLTI1MC5tZW1iZXJzLmxpbm9kZS5jb20wHhcNMTQxMTE0MTAyODA0
WhcNMjQxMTExMTAyODA0WjAnMSUwIwYDVQQDExxsaTE5MC0yNTAubWVtYmVycy5s
aW5vZGUuY29tMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAsKmy9o5z
WU+1EH7Z3bB5TGQA+16zXDcEJy6tZWZ8CDrRyQXiahendp45BWUc/ZuLDo0+B3Wt
ZXjofmLw/F4fmR+8X1s1fQZX2dFt920qEm7LxqzWd0c7FdHiBwwRrwhkk+3cQpOB
TTGdLWEgpdmwwNZDTUdsDLzjDczPnju6T6p6ArTECztPbmTjfY4QIRtC6capL1Z+
yPJSQVAuAMEX1wTDWTGdm0VV7oW4F5cGZutf6QAP51jdhSyZuGilIPHbnj0l6Qc7
a7+OtEsEGi31aJ8KpRf7LNZ7DXCuoB3Hf75Pd6VjDgoOIagcH0NYqa75gEjBkGzs
ktLWykT7ag7fKwIDAQABow0wCzAJBgNVHRMEAjAAMA0GCSqGSIb3DQEBCwUAA4IB
AQCaZdUNAj8WDEKWdoU3LNXUBJlTJwiWBrh550PbHSQORcCz2K0kiMei1A4ojK2N
dMHFGAqAeUEaxtz92p2BoFpZasAtdSa3u63tBckFhfUolIS1TC7Cj51y19ysTeep
fGPFpuPCVqVPsruei8Z/iqn3bFIhQQdmumeePZQdPMwZSWHNVYC5XODd7PvNDrDu
5MZJjkz4+6LbwwAvyew62meFN2QEsYbK2Brtbhze+IjE27FGWlSw4K3jlwa409MD
MTf4JU41ELaYY8G/LSNDJsBVhhkHzvXR9iCbXxNz3IL0dQDNj7h4LKhBy0q7hvqg
kDzwlmBO4WKSmCAuky44cXmd
-----END CERTIFICATE-----
subject=/CN=li190-250.members.linode.com
issuer=/CN=li190-250.members.linode.com
---
No client certificate CA names sent
---
SSL handshake has read 1714 bytes and written 637 bytes
---
New, TLSv1/SSLv3, Cipher is DHE-RSA-AES256-SHA
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
SSL-Session:
    Protocol  : SSLv3
    Cipher    : DHE-RSA-AES256-SHA
    Session-ID: 0654B95D0F15B41F0DA5FF4B770E463876D3A127986A3F19F33279681855BFC4
    Session-ID-ctx:
    Master-Key: F5ACD2C5078D2CA7D607EAB01B16B546F2AA9614769A0E0E2539771293E6CC9F97997664FA2D35D9D9A14C80163736A8
    Key-Arg   : None
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    Start Time: 1475210540
    Timeout   : 300 (sec)
    Verify return code: 18 (self signed certificate)
---
<bandit15_password and return key>
Correct!
<bandit16_password>

read:errno=0

```


## Level 16
**Description**
The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.


**Solution**
First I scanned for open ports from 31000 to 32000 in nmap. It didn't give me any services so I tried the sV flag, which takes a little longer.
```bash
bandit16@melinda:~$ nmap -sT -p31000-32000 localhost

Starting Nmap 6.40 ( http://nmap.org ) at 2016-09-30 04:47 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00047s latency).
Not shown: 995 closed ports
PORT      STATE SERVICE
31046/tcp open  unknown
31099/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.08 seconds

bandit16@melinda:~$ nmap -sV -p31000-32000 localhost

Starting Nmap 6.40 ( http://nmap.org ) at 2016-09-30 04:53 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00049s latency).
Not shown: 995 closed ports
PORT      STATE SERVICE VERSION
31046/tcp open  echo
31099/tcp open  ssh     (protocol 2.0)
31518/tcp open  msdtc   Microsoft Distributed Transaction Coordinator (error)
31691/tcp open  echo
31790/tcp open  msdtc   Microsoft Distributed Transaction Coordinator (error)
31960/tcp open  echo
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at http://www.insecure.org/cgi-bin/servicefp-submit.cgi :
SF-Port31099-TCP:V=6.40%I=7%D=9/30%Time=57EDEFDB%P=x86_64-pc-linux-gnu%r(N
SF:ULL,2B,"SSH-2\.0-OpenSSH_6\.6\.1p1\x20Ubuntu-2ubuntu2\.8\r\n");
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at http://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 41.23 seconds

```

This shows echo on several ports which we can ignore. That leaves an ssh port and two msdtc ports. Doing a quick echo to netcat on those ports it kindly tells me that they are ssl ports.
```bash
bandit16@melinda:~$ echo '<bandit16_password>' | nc localhost 31518
ERROR
140737354045088:error:1408F10B:SSL routines:SSL3_GET_RECORD:wrong version number:s3_pkt.c:350:
bandit16@melinda:~$ echo '<bandit16_password>' | nc localhost 31790
ERROR
140737354045088:error:1408F10B:SSL routines:SSL3_GET_RECORD:wrong version number:s3_pkt.c:350:
```

Trying the ssl ports first, I found the one I wanted in port 31790.

```bash

bandit16@melinda:~$ openssl s_client -connect localhost:31790 -ign_eof
CONNECTED(00000003)
depth=0 CN = li190-250.members.linode.com
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = li190-250.members.linode.com
verify return:1
---
Certificate chain
 0 s:/CN=li190-250.members.linode.com
   i:/CN=li190-250.members.linode.com
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIC3jCCAcagAwIBAgIJAI5QiWZw4YHbMA0GCSqGSIb3DQEBCwUAMCcxJTAjBgNV
BAMTHGxpMTkwLTI1MC5tZW1iZXJzLmxpbm9kZS5jb20wHhcNMTQxMTE0MTAyODA0
WhcNMjQxMTExMTAyODA0WjAnMSUwIwYDVQQDExxsaTE5MC0yNTAubWVtYmVycy5s
aW5vZGUuY29tMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAsKmy9o5z
WU+1EH7Z3bB5TGQA+16zXDcEJy6tZWZ8CDrRyQXiahendp45BWUc/ZuLDo0+B3Wt
ZXjofmLw/F4fmR+8X1s1fQZX2dFt920qEm7LxqzWd0c7FdHiBwwRrwhkk+3cQpOB
TTGdLWEgpdmwwNZDTUdsDLzjDczPnju6T6p6ArTECztPbmTjfY4QIRtC6capL1Z+
yPJSQVAuAMEX1wTDWTGdm0VV7oW4F5cGZutf6QAP51jdhSyZuGilIPHbnj0l6Qc7
a7+OtEsEGi31aJ8KpRf7LNZ7DXCuoB3Hf75Pd6VjDgoOIagcH0NYqa75gEjBkGzs
ktLWykT7ag7fKwIDAQABow0wCzAJBgNVHRMEAjAAMA0GCSqGSIb3DQEBCwUAA4IB
AQCaZdUNAj8WDEKWdoU3LNXUBJlTJwiWBrh550PbHSQORcCz2K0kiMei1A4ojK2N
dMHFGAqAeUEaxtz92p2BoFpZasAtdSa3u63tBckFhfUolIS1TC7Cj51y19ysTeep
fGPFpuPCVqVPsruei8Z/iqn3bFIhQQdmumeePZQdPMwZSWHNVYC5XODd7PvNDrDu
5MZJjkz4+6LbwwAvyew62meFN2QEsYbK2Brtbhze+IjE27FGWlSw4K3jlwa409MD
MTf4JU41ELaYY8G/LSNDJsBVhhkHzvXR9iCbXxNz3IL0dQDNj7h4LKhBy0q7hvqg
kDzwlmBO4WKSmCAuky44cXmd
-----END CERTIFICATE-----
subject=/CN=li190-250.members.linode.com
issuer=/CN=li190-250.members.linode.com
---
No client certificate CA names sent
---
SSL handshake has read 1714 bytes and written 637 bytes
---
New, TLSv1/SSLv3, Cipher is DHE-RSA-AES256-SHA
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
SSL-Session:
    Protocol  : SSLv3
    Cipher    : DHE-RSA-AES256-SHA
    Session-ID: EAF0BEFEA9B261E1540AA2F06C3535E618B77FE9439C20F9B5C5980BC8E32F4C
    Session-ID-ctx:
    Master-Key: 38DF8099B20A8506E7F176242656E5741FA69EF258A7AFE1B18257C2C456161D3D35A9037FF6848EDF96E49F657F5336
    Key-Arg   : None
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    Start Time: 1475211616
    Timeout   : 300 (sec)
    Verify return code: 18 (self signed certificate)
---
<bandit16_password>
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

read:errno=0
```

Next I just copied this text into a file in the working directory in my own computer, logged into level 17, and got the password to level 17 from the /etc/bandit_pass/bandit17 file.

```bash
> vi new_key.pass
> chmod 600 new_key.pass
> ssh -i new_key.pass bandit17@bandit.labs.overthewire.org
bandit17@melinda:~$ cat /etc/bandit_pass/bandit17
<password>
```


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


## Level 18
**Description**
The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

**Solution**
Once again, there's probably another way you're supposed to do this, but I just grabbed the file with secure copy.

```bash
> scp bandit18@bandit.labs.overthewire.org/~/readme .
> cat ./readme
<password>
```

## Level 19
**Description**
To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used to setuid binary.

**Solution**
With an ll it looks like there is a file that can run as bandit20. I ran the file and it told me what commands to use. I tried a whoami to find out what user it ran as, and then proceeded to perform a cat on the bandit password file.

```bash
bandit19@melinda:~$ ll
total 28
drwxr-xr-x   2 root     root     4096 Nov 14  2014 ./
drwxr-xr-x 172 root     root     4096 Jul 10 14:12 ../
-rw-r--r--   1 root     root      220 Apr  9  2014 .bash_logout
-rw-r--r--   1 root     root     3637 Apr  9  2014 .bashrc
-rw-r--r--   1 root     root      675 Apr  9  2014 .profile
-rwsr-x---   1 bandit20 bandit19 7370 Nov 14  2014 bandit20-do*
bandit19@melinda:~$ ./bandit20-do
Run a command as another user.
  Example: ./bandit20-do id
bandit19@melinda:~$ ./bandit20-do whoami
bandit20
bandit19@melinda:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
<password>
```

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

## Level 21
**Description**
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

**Solution**
I looked in the cron directory and found a file called bandit22. After looking at this file, I saw it ran a script in usr/bin. Viewing the script I saw that it copied the password to a file in the tmp directory. Viewing this file, I found the next password.

```bash
bandit21@melinda:~$ cd /etc/cron.d
bandit21@melinda:/etc/cron.d$ ls
behemoth4_cleanup  cronjob_bandit23       leviathan5_cleanup    natas-session-toucher  natas25_cleanup~  php5        semtex0-ppc  vortex0
cron-apt           cronjob_bandit24       manpage3_resetpw_job  natas-stats            natas26_cleanup   semtex0-32  semtex5      vortex20
cronjob_bandit22   cronjob_bandit24_root  melinda-stats         natas25_cleanup        natas27_cleanup   semtex0-64  sysstat
bandit21@melinda:/etc/cron.d$ cat cronjob_bandit22
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
bandit21@melinda:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@melinda:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
<password>
```



## Level 22
**Description**
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

**Solution**
From the previous level, I remember a cronjob_bandit23 in the ect/cron.d folder so I cat the file. It shows me the cronjob shell script that runs. Reading the shell script I can tell that the password is copied into a file in the tmp directory. This filename is the md5sum of the text "I am user bandit23". After finding that out, it's as simple as viewing the file, and there I found the password for bandit 23.

```bash
bandit22@melinda:/etc/cron.d$ cat cronjob_bandit23
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@melinda:/etc/cron.d$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
bandit22@melinda:/etc/cron.d$ echo I am user bandit23 | md5sum
8ca319486bfbbc3663ea0fbe81326349  -
bandit22@melinda:/etc/cron.d$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
<password>
```


## Level 23
**Description**
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

**Solution**
The bandit24 cronjob script is running under /usr/bin/cronjob_bandit24. Viewing the script, it appears that the script executes and deletes all scripts in the /var/spool/bandit24 directory. 

```bash
bandit23@melinda:/etc/cron.d$ cat cronjob_bandit24 
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null

bandit23@melinda:/etc/cron.d$ cat /usr/bin/cronjob_bandit24.sh 
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname
echo "Executing and deleting all scripts in /var/spool/$myname:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
	echo "Handling $i"
	timeout -s 9 60 "./$i"
	rm -f "./$i"
    fi
done

```

It looks like I can just write a script that gets the password and writes it into another file I can view. I wrote the following script and copied it into the /var/spool/bandit24 directory.

```bash
bandit23@melinda:/tmp/temp0000098$ touch getpass.pl
bandit23@melinda:/tmp/temp0000098$ touch output.txt
bandit23@melinda:/tmp/temp0000098$ chmod 777 *
bandit23@melinda:/tmp/temp0000098$ vi getpass.pl
bandit23@melinda:/tmp/temp0000098$ cp getpass.pl /var/spool/bandit24/getpass.pl
```

```perl
#!/usr/bin/perl 
system('cat /etc/bandit_pass/bandit23 > /tmp/temp0000098/output.txt');
```

Then all was left to let the cronjob run and cat the file in my temp directory.

```bash
bandit23@melinda:/tmp/meohmy$ cat output.txt 
<password>
```




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


## Level 25
**Description**
Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.


**Solution**
Logging in there is an ssh key to the next level. So I grabbed that to my own box and chmod 600 the permissions, and tried to log in. It immediatly logged me out. (Worth a shot)

```bash
bandit25@melinda:~$ ls
bandit26.sshkey
> scp bandit25@bandit.labs.overthewire.org:~/bandit26.sshkey .
> chmod 600 bandit26.sshkey
> ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org
```

Since the hint was the user is not /bin/bash I tried to figure out what it was by viewing the passwd file. It was using some script called showtext.
```bash
bandit25@melinda:~$ cat /etc/passwd |grep bandit
bandit0:x:11000:11000:bandit level 0:/home/bandit0:/bin/bash
bandit1:x:11001:11001:bandit level 1:/home/bandit1:/bin/bash
bandit2:x:11002:11002:bandit level 2:/home/bandit2:/bin/bash
bandit3:x:11003:11003:bandit level 3:/home/bandit3:/bin/bash
bandit4:x:11004:11004:bandit level 4:/home/bandit4:/bin/bash
bandit5:x:11005:11005:bandit level 5:/home/bandit5:/bin/bash
bandit6:x:11006:11006:bandit level 6:/home/bandit6:/bin/bash
bandit7:x:11007:11007:bandit level 7:/home/bandit7:/bin/bash
bandit8:x:11008:11008:bandit level 8:/home/bandit8:/bin/bash
bandit9:x:11009:11009:bandit level 9:/home/bandit9:/bin/bash
bandit10:x:11010:11010:bandit level 10:/home/bandit10:/bin/bash
bandit11:x:11011:11011:bandit level 11:/home/bandit11:/bin/bash
bandit12:x:11012:11012:bandit level 12:/home/bandit12:/bin/bash
bandit13:x:11013:11013:bandit level 13:/home/bandit13:/bin/bash
bandit14:x:11014:11014:bandit level 14:/home/bandit14:/bin/bash
bandit15:x:11015:11015:bandit level 15:/home/bandit15:/bin/bash
bandit17:x:11017:11017:bandit level 17:/home/bandit17:/bin/bash
bandit18:x:11018:11018:bandit level 18:/home/bandit18:/bin/bash
bandit19:x:11019:11019:bandit level 19:/home/bandit19:/bin/bash
bandit20:x:11020:11020:bandit level 20:/home/bandit20:/bin/bash
bandit21:x:11021:11021:bandit level 21:/home/bandit21:/bin/bash
bandit22:x:11022:11022:bandit level 22:/home/bandit22:/bin/bash
bandit23:x:11023:11023:bandit level 23:/home/bandit23:/bin/bash
bandit25:x:11025:11025:bandit level 25:/home/bandit25:/bin/bash
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
bandit24:x:11024:11024:bandit level 24:/home/bandit24:/bin/bash
bandit16:x:11016:11016:bandit level 16:/home/bandit16:/bin/bash
```

Viewing the showtext file, I see it runs a more on this text.txt file and then exits. I looked to see if I could edit this file, but root owns it. I then looked to see if I could edit the text.txt file, but no luck.
```bash
bandit25@melinda:~$ cat /usr/bin/showtext 
#!/bin/sh

more ~/text.txt
exit 0

bandit25@melinda:~$ ls -l /usr/bin/showtext 
-rwxr-xr-x 1 root root 34 Nov 16  2014 /usr/bin/showtext

bandit25@melinda:~$ ls -al ../bandit26/
total 32
drwxr-xr-x   3 root     root     4096 Nov 16  2014 .
drwxr-xr-x 172 root     root     4096 Jul 10 14:12 ..
-rw-r--r--   1 root     root      220 Apr  9  2014 .bash_logout
-rw-r--r--   1 root     root     3637 Apr  9  2014 .bashrc
-rw-r--r--   1 root     root      675 Apr  9  2014 .profile
drwxr-xr-x   2 root     root     4096 Nov 16  2014 .ssh
-rw-------   1 bandit26 bandit26  430 Nov 16  2014 README.txt
-rw-r-----   1 bandit26 bandit26  258 Nov 16  2014 text.txt

```

It's also so small that the more command wouldn't be needed, so maybe there was something I could use there. I wish I could say I knew the answer right away, but I had to read the man for more, and you can change to vi in visual move by pressing the 'v' button. At this point, I'm able to run other vi commands like... :r /etc/bandit_pass/bandit26 to read the password.

```bash
> ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org
> v                     _ _ _   ___   __
 | |                   | (_) | |__ \ / /
 | |__   __ _ _ __   __| |_| |_   ) / /_
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/
> :r /etc/bandit_pass/bandit26
<password>
```


## Level 26
**Description**
N/A

**Solution**
All that was left is to read that README.txt file we saw in the bandit26 directory
```bash
> ssh bandit26@bandit.labs.overthewire.org
> v                     _ _ _   ___   __
 | |                   | (_) | |__ \ / /
 | |__   __ _ _ __   __| |_| |_   ) / /_
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/
> :r ./README.txt
Congratulations on solving the last level of this game!

At this moment, there are no more levels to play in this game. However, we are constantly working
on new levels and will most likely expand this game with more levels soon.
Keep an eye out for an announcement on our usual communication channels!
In the meantime, you could play some of our other wargames.

If you have an idea for an awesome new level, please let us know!
```
