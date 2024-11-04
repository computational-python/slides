# match

## on the new switch  in Python

### Course

KTH

---

layout: false

## The switch scenario in Python has been `if...elif...```


~~~
if thing == 'foo':
    ...
elif thing == 'bar'::
    ...
elif thing == 'baz'::
    ...
~~~

---
All languages have some if-statement  structure and most have a 
branching switch case

Example

bash

```bash
arg=${1-none}
if [ $arg == foo ]; then
    echo 'handling foo'
elif [ $arg == bar ]; then
    echo 'handling bar'
else
    echo "don't know how to handle $arg"
fi
```

~~~bash
arg=${1-none}
case $arg in
    foo) echo 'handling foo' ;;
    bar) echo 'handling bar' ;;
    *) echo "don't know how to handle $arg" ;;
esac
~~~
---

## Come 3.10

a new switch like structure

~~~
match thing:
    case 'foo':
        ...
    case 'bar':
        ...
    case 'baz':
        ...
~~~


