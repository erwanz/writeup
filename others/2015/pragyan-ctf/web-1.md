### Solved by NullMode

This challenge consisted of downloading a zip file containing what appeared to be a web application.

I started looking through the files source code. Eventually I got to aes.js and an MD5 string caught my attention....

    $('#flag').html("904d553eae0a2a5b82d82fd4f0c7ae6f5fe955f5");

I thought this was probably too good to be true but I submitted **904d553eae0a2a5b82d82fd4f0c7ae6f5fe955f5** anyway...

Turns out it was was the flag! Very easy 20 points!
