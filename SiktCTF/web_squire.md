# All that for a SQL injection
This challenge taugh me about, werkzeug, flask and JWT, and it was not suppose to!

The description in this task is quite elaberate, and suggest that the solution is found in a logical error, and not
by brute force, so no hydra!

I tried a sql injection first, and as we will see it was the solution, but I might have missed out on url encoding, or
there might have been a type because it did not stick.

I therefore got obsessed with the reference to structure in the description and was convinced the solution is somewhere in
the stack, i.e werkzeug, and therefore most likely flask. I started reading about a lot of werkzeug vulnerabilities,
and was looking for the werkzeug console page. This did not work, so I switched over to what I thought was an JWT token
vulnerability, but flask does not nativly use those, insted it uses a flask session cookie, with similar structure.

So what I thought was a missformed JWT token header was just a flask session cookie. No go there either.

During my attempt something strange had happend some times, the "Login failed" message had been replicated on some requests,
appering twice. Thinking about this it struck me that that is obviously something that need checking. I opened Caido, and
sendt a request, trying the reliable ' character. In Caido I saw that this was encoded, and by sending an encoded version of
the sql injection `username=admin&password=%27+OR+%271%27%3D%271` it responded with a login success!

Note:
I never tested if it was the encoding bit that was initially wrong, or if I just had a typo.



