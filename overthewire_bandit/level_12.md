# Over the Wire Bandit Walkthrough (Writeup): http://overthewire.org/wargames/bandit/

## Writeup

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
