# All that for a SQL injection
This challenge taugh me about, werkzeug, flask and JWT, and it was not suppose to!

The description in this task is quite elaberate, and suggest that the solution is found in a logical error, and not
by brute force, so no hydra!

I tried a sql injection first, and as we will see that is the solution, but I might have missed out on url encoding, or
there might have been a type because it did not stick.

I therefor got obsesed with the reference to structure in the discription and was convinsed the solution is somewhere in
the stack, i.e werkzeug, and therefor most likely flask. I therefor started reading about a lot of werkzeug vulnerabilities, 
and was looking for the werkzeug console page. This did not work, so I switched over to what I taught was an JWT token
vulnarability, but flask does not nativly use those, insted it uses a flask session cookie, with similar structure.

So what I taught was a misformed JWT token header was just a flask session cookie. No go there either.

During my attempt something strange had happend some times, the "Login failed" massage had been replicated on some requests,
appering twice. Thinking about this it struck me that that is obviously somthing that need checking. I opened caido, and
sendt a request, trying the reliable ' character. In caido I saw that this was encoded, and by sending an encoded version of
the sql injection `username=admin&password=%27+OR+%271%27%3D%271` it responded with a login success!

I desided to write this one more as a story and less as a how to, because even though I'm familiar with sql injection trough
burp training and TryHackMe, I initially failed at recognising it in the wild, even ignoring some clues in persuit of a
much harder solution. This might be relevent to others as well.


Note:
I never tested if it was the encoding bit that was initially wrong, or if I just had a typo.



