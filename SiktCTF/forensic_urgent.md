# Urgent
This was a fun one, but really not a lot of forensic in this task.

First enter the ssh server using
```bash
$ ssh ctfuser@challenges.ctf.sikt.no -p 2200
```

In there if you run a ```ls -la``` you will see the .logs folder. This folder contains a auth.log file,
run ```cat .logs/auth.logs``` and in the log you will see someone trying to log in from port 2121.
This is a major hint, if you try to connect to port 2121 using netcat you will see an unsuprising banner.
```bash
$ nc challenges.ctf.sikt.no 2121
220 (vsFTP 3.0.5)
```

FTP usually runs on port 21, given that the task is to recover the file and that FTP is the file transfer protocol, I say
we are on the right track.

On my mac the only ftp client I have is a sftp one, I am unfamiliar with how it works with regular ftp, but I
do have a vm running debian. Using the VM and the regular old ftp client I was able to connect and get the flag.
```bash
$ ftp challenges.ctf.sikt.no -p 2121
...
Name: <type anonymous here>
230 Login successful.
...

ftp> ls
<here you see a flag file>

ftp> get <name of flag file>
...

ftp> exit
221 Goodbye.
```

The file is now in the folder you accessed ftp in.


