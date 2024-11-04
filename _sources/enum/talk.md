# enum

## enums in Python

### 


---

layout: false

---

## Slide

>  An enumeration is a set of symbolic names (members) bound to unique, constant
>  values. Within an enumeration, the members can be compared by identity, and
>  the enumeration itself can be iterated over.

https://docs.python.org/3/library/enum.html

~~~
>>> from enum import Enum
>>> Card = Enum('Card', 'HEARTS DIAMONDS CLUBS SPADES')
>>> Card
<enum 'Card'>
>>> list(Card)
[<Card.HEARTS: 1>, <Card.DIAMONDS: 2>, <Card.CLUBS: 3>, <Card.SPADES: 4>]

~~~

---

~~~
>>> class Card(Enum):
...     HEARTS = 10
...     DIAMONDS = 20
...     CLUBS = 30
...     SPADES = 40
>>> Card
<enum 'Card'>
>>> list(Card)
[<Card.HEARTS: 10>, <Card.DIAMONDS: 20>, <Card.CLUBS: 30>, <Card.SPADES: 40>]

~~~

~~~
>>> Card.HEARTS.value
10

~~~

---

~~~
>>> from enum import auto
>>> class Card(Enum):
...     HEARTS = auto()
...     DIAMONDS = auto()
...     CLUBS = auto()
...     SPADES = auto()
>>> Card
<enum 'Card'>
>>> list(Card)
[<Card.HEARTS: 1>, <Card.DIAMONDS: 2>, <Card.CLUBS: 3>, <Card.SPADES: 4>]

~~~
