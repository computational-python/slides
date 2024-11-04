## The walrus operator `:=`


<img src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Ftse3.mm.bing.net%2Fth%3Fid%3DOIP.hDoxkZv73s3s2m0sO_mUJQHaFA%26pid%3DApi&f=1">

---

layout: false

## The walrus operator `:=`

Introduced in Python 3.10 to handle some particular situations

### Example
Search for a pattern in a string then go on to handle the match, such
as searching for an email address between brackets `<>`

~~~
>>> import re
>>> match = re.search(r'.*<(.*)>', "John Doe <john@google.com>")
>>> if match:
...     print(match.group(1))
john@google.com

~~~

Would it be possible to put the search line directly in the if statement?
If we use the walrus operator

~~~
>>> if match := re.search(r'.*<(.*)>', "John Doe <john@google.com>"):
...     print(match.group(1))
john@google.com

~~~

---

An assignment where the value of the assignment expression has the same value

~~~
>>> print(x := 8)
8
>>> print(x)
8

~~~
