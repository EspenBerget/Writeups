# A Pure Programming challenge

I would not be much of a hacker if I did not give this one lot of garbage input first, but non sticked.
No %x crash or leak, no overflow, either stack or integer. After a good go I desided that the task
looked easy enough to automate.

The task is a snake game, but the tail never growes. This makes for a trivial tactic, simply traverse the entire game up and
down like a typewritter.

With a bit of back and fort I was able to produce this python script based on pwntools.
```python
from pwn import *

io = remote("challenges.ctf.sikt.no", 5010)
io.sendline("Y")

def up():
    io.sendline(b"1")

def down():
    io.sendline(b"2")

def left():
    io.sendline(b"3")

def right():
    io.sendline(b"4")


def step(n, action):
    for i in range(n):
        action()

# set start position to top left corner
step(8, up)
step(8, left)

def proc_down():
    step(15, right)
    down()
    step(15, left)
    down()


def proc_up():
    step(15, left)
    up()
    step(15, right)
    up()

try:
    while True:
        step(7, proc_down)
        step(15, right)
        down()
        step(7, proc_up)
        step(15, left)
        up()
        log.info(io.clean())

except:
    print("pip broken")
```

The approch here is fairly basic. Simply start the game with a Y, define the moves as functions for readability and mnemonics.
Move the snake to the top corner (might even get lucky here and get some hits). Finally, traverse the whole screen until the
game is satisfied. This happend on 100 point, the flag was printed after that.


