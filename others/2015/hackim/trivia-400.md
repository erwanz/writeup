### Solved by et0x

This challenge presents the following hint for you to follow:

`For a n-gram where n=32 find the number with highest frequency in the string, given in the image. Find the synonym of this string.`

![](/images/2015/hackim/trivia400/1.png)

The first thing I do to convert this image to text is throw this into an OCR engine online.  In this case I used http://www.free-ocr.com/.

After researching some information on n-grams, I see that the way ngrams work by taking n items after a set point in some text, and return the block as an ngram.
For example, if you had an an n-gram of "abcdefgh" where n is equal to 3, your n grams would be "abc", "bcd", "cde", "def", "efg", "fgh".   As far as I could tell, n grams are used largely
for analyzing languages in a programmatic fashion.  In our case, this will simply find every single 32-bit combination in our text, return it's decimal value, and then compare
these returned numbers to find the most frequently occurring one.  This most frequently occurring number is somehow our "synonym" to the actual flag.    I devise a script which
takes care of all of the above tasks, and spits out the decimal number, which is converted to hex, then from hex to ASCII.

The script is as follows:

```python
import itertools, operator

def most_common(L):           #find most frequently occuring item in a list
  groups = itertools.groupby(sorted(L))
  def _auxfun((item, iterable)):
    return len(list(iterable)), -L.index(item)
  return max(groups, key=_auxfun)[0]


def printNGrams(list):
        nList = []
        for y in list:
                nList.append(int(y,2))
        return nList

intDict = {}

a = open("400-txt","r").read()
z = " ".join(a[x:x+32] for x in range(0,len(a)-31)).split() #split ngrams
print hex(most_common(printNGrams(z)))[2:].decode("hex")
```
Upon running the script, our text is neatly returned:

```
root@kali:/ctfs/nullcon 2015/trivia# python trivia400.py 
evil
```

So the flag will be a synonym to the word "evil".  Now the next part was primarily a guessing game, I just went to thesaurus.com, and plugged in "evil" as our base word, then tried every possible
combination of the words that were returned.  In the end our flag ended up being `maleficence`  !

