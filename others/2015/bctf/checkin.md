### Solved by superkojiman

We joined in BCTF at the last minute, but managed to solve 3 challenges. Checkin was basically a freebie. The hint was to check-in at the #bctf IRC channel on Freenode. In doing so I noticed the topic had the following string: OPGS{jr1p0zr-g0-OPGS-2015_t00q-yhpx}

That was obviously the flag, but encrypted in some form. In this case, it was ROT13: 

```
# python -c 'print "OPGS{jr1p0zr-g0-OPGS-2015_t00q-yhpx}".decode("rot13")'
BCTF{we1c0me-t0-BCTF-2015_g00d-luck}
```
