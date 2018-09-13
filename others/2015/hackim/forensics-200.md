### Solved by et0x

This challenge presents the following hint for you to follow:

`A friend of mine says his SSH server is super secure. Can you pls help me find the hidden message ?`

`54.165.191.231:2222`

Upon sshing to the server, if you use the -v switch, you see that there is a strange banner for the version of ssh running.

```
debug1: match: OpenSSH_4.2p1 Debian-7ubuntu3.1#SSH-2.0-OpenSSH_5.1p1 Debian-5 pat OpenSSH_4*
```

Also, when you try to login, you don't get the typical "root@ip's password: " prompt.  You get a "password: " prompt, and if you enter three incorrect password, it goes to the typical prompt.  So it looks like there are two different ssh servers running.  This is irrelevant really, as the hint for this challenge states the following:

 `<!-- 1.Server is strong, but his password is not..... try knocking him gently. 2.Are you using right way to connect to me? -->`

So long story short, after trying many passwords, the correct one ends up being **nullcon**.  

Upon logging in and doing some information gathering of the system, a suspicious looking folder: "/tmp/s.e.c.r.e.t" is found.

Within this folder there are many different files, but one is especially interesting, as it contains what looks like a BrainFuck Program.

```
root@nullcon_db_svr02:/tmp/s.e.c.r.e.t# cat new.b
+++++ +++++ [->++ +++++ +++<] >++.+ +++++ .<+++ [->-- -<]>- -.+++ +++.<
++++[ ->+++ +<]>+ +++.< +++[- >---< ]>--- -.<++ +++++ [->-- ----- <]>--
----- ---.< +++++ +++[- >++++ ++++< ]>+++ +.<++ ++[-> ----< ]>--- -----
.<+++ [->++ +<]>+ +++.< +++[- >---< ]>--. <+++[ ->+++ <]>++ ++.-- -----
.<+++ [->++ +<]>+ ++++. <++++ [->-- --<]> ----. +++++ +.--. <+++[ ->+++
<]>++ +++.< ++++[ ->--- -<]>- ---.+ +.<++ ++[-> ++++< ]>+.< +++++ ++[->
----- --<]> ----- ----- ----. <++++ ++[-> +++++ +<]>+ +++++ ++.<+ ++[->
+++<] >++++ ++.<+ +++++ +[->- ----- -<]>- ----- ----. .<+++ ++++[ ->+++
++++< ]>.+. -.<++ +++[- >++++ +<]>. <++++ +++++ +[->- ----- ----< ]>---
----- ----. ---.< 
```

After throwing this into a brainfuck interpreter, the output is the flag!

`flag{n3w_languages_ar3_n33ded}`

For those curious of how brainfuck works, the following link explains it well: http://en.wikipedia.org/wiki/Brainfuck

Also, I've made an animation to help people get the jist of how the different operators work.  Hope this helps someone!

![](/images/2015/hackim/forensics200/bf-explanation.gif)
