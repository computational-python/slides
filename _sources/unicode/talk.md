# What is unicode?

note: need to consider incode vs ondisk difference,see Eric B

---

layout: false

## Characters for humans

Graphical building blocks of a written language

* Letters: a, b, π, の
* Digits: $(0, 1...)$
* Emojis: ☺, ◐, ◑, ♫

## Characters for computers: bits

* all information in computers are stored as bits
* we need standardized tables to translate bits to characters

### ASCII

* American Standard Code for Information Interchange
* one of the first tables - based on 7 bits
* $2^7 = 128$ possible combinations
* 128 characters considered enough for the English language
* 'A' has a bit representation 1000001 (decimal 65)

### Early nationalized versions

* For languages with non-english characters som local changes were made to the table
* In Swedish `][\` were replaced by `ÅÄÖ`
    - you could see array expressions displayed as `Ä1,2,3Å`
    - the reson why Fortran still has the clumsy array syntax - `(/1,2,3/)`
*

<img src="ascii.jpeg">

### latin-1

* 8 bits (1 byte) per character
* allows for a wider class of characters, mostly european
* such as åäö

### utf-8 and unicode

## encode/decode

~~~
>>> 'A'.encode()
b'A'
>>> type('A'.encode())
<class 'bytes'>
>>> 'A'.encode().decode()
'A'
>>> type('A'.encode().decode())
<class 'str'>
~~~

---



