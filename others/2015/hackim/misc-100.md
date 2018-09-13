### Solved by superkojiman

A 100 point challenge with the following description: 

> Talk to me pls @ IP PORT - 54.165.191.231:2908 

Attempting to connect to this target would result in a message saying that we were coming from the wrong location. As it turns out, we need to connect to the target with an IP address in China. I used a free proxy server located in China and added it to my proxychains.conf file: 

```
http    123.125.19.44   80
```

I attempted to connect and was presented with an "attack" menu:


```
# proxychains4 nc 54.165.191.231 2908
[proxychains] config file found: /usr/local/etc/proxychains.conf
[proxychains] preloading /usr/local/lib/libproxychains4.so
[proxychains] DLL init: proxychains-ng 4.8.1
[proxychains] Strict chain  ...  123.125.19.44:80  ...  54.165.191.231:2908  ...  OK


Yes, now lets start attacking ...This would divert all the investigations ;)
------------------------------
   ATTACK - M E N U
   ------------------------------
   1. Launch DNS attack
   2. Launch DOS 
   3. Launch PoD
   4. Launch HailMary
   5. Launch Sniffers
   6. Launch traffic hijack
   7. Launch Perimeter attacks
   8. Exit ATTACK MENU
   ------------------------------
   Enter your choice [1-8] : 
```

After trying out the options, only option 8 returned the flag:

```
# proxychains4 nc 54.165.191.231 2908 << EOF
8
EOF
[proxychains] config file found: /usr/local/etc/proxychains.conf
[proxychains] preloading /usr/local/lib/libproxychains4.so
[proxychains] DLL init: proxychains-ng 4.8.1
[proxychains] Strict chain  ...  123.125.19.44:80  ...  54.165.191.231:2908  ...  OK


Yes, now lets start attacking ...This would divert all the investigations ;)
------------------------------
   ATTACK - M E N U
   ------------------------------
   1. Launch DNS attack
   2. Launch DOS 
   3. Launch PoD
   4. Launch HailMary
   5. Launch Sniffers
   6. Launch traffic hijack
   7. Launch Perimeter attacks
   8. Exit ATTACK MENU
   ------------------------------
   Enter your choice [1-8] : flag{Tor!s-useful}
```

The flag is **flag{Tor!s-useful}**

