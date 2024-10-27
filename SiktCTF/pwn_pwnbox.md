# Pwnbox WriteUp
A classical pwn challenge. This one involved scanning, web traversary, leaked passwords, and git.

Start of by finding the open ports. This is easy with nmap `nmap -T2 -p7000-8000 challenges.ctf.sikt.no` just to be nice I
used a slower scan with -T2, might not have been a problem, but I was in no hurry.

After finding the open ports I used netcat to identify them, I often find this to be a good way to get a quick answer to
what some open port is.

The first port 7203 does not respond, but by typing "GET / HTTP/1.1" I get a http response. So this is a web server.
The second port 7204 immidiatly response with a "SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.5", an ssh server.

Visiting the website with a browser, I saw that a file upload page. I initialy spendt a lot of time here trying to find the
file upload vulnerability, but the answer is a lot eastier. By uploading any file you will see where it is uploaded.

If you don't have a image you can easily make one in the shell with `convert -size 80x80 xc:white empty.png`.

After uploading an image I clicked the link to go to the image, then I removed the image part and put any non existing
filename in its place. This presented me with a page that listed the files on the server. There I saw a readme.txt file.
Visiting this file I saw this text. 
```
Welcome to the webserver git repository!

Remember to delete this file before starting the server! Dont leak the server password!
Username: dustin   Password: <dustin pass>
```

Now I logged into the server using the ssh port, and dustins credentials.

In the server there is a repo folder in /home/dustin. It contains the readme.txt from earlier. With `git log` in the repo I
saw that one commit contains admin credentials. A simple `git checkout 307b` gave me a new version of the readme.txt file.
```bash
$ cat readme.txt

Welcome to the repository!
Remember to delete this file before starting the server! Dont leak the server password!
Username: admin\tPassword: <admin pass>
```

I now ssh'ed into the server again, with admin as username. I could probably have used `su` as well.

In the admin home folder there is a flag.txt, using cat I got the flag.


