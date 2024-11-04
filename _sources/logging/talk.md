# logging

## an alternative print

### info


---

layout: false

## why not print?

* not bad
* sometimes debuggers are better
* monitor over time
* keep print around?
* mix production and debug output


---

## the logging module

Part of the standard library

~~~
>>> import logging

~~~

<!--
~~~
>>> import sys
>>> logging.basicConfig(stream=sys.stdout)

~~~
-->

Contains some standard (adaptable) functions to be put in calls

~~~
>>> logging.error('This must clearly be wrong')
ERROR:root:This must clearly be wrong

~~~

---

## logging functions /levels

levels of importance

* critical
* error
* warning
* info
* debug

~~~
>>> logging.critical('Abort abort!')
CRITICAL:root:Abort abort!
>>> logging.error('This must clearly be wrong')
ERROR:root:This must clearly be wrong
>>> logging.warning('This does not seem right')
WARNING:root:This does not seem right
>>> logging.info('In case you would like to know')
>>> logging.debug(f'Detail: current value...')

~~~

The set level determines what is printed. 

Default is 'warning' - only warning
and higher levels are printed.

---

Levels initialized by

~~~
>>> logging.basicConfig(level=logging.INFO)

~~~

We can further

* categorize output
* adjust print level
* redirect output

---

### String Formatting

convention to use older type of formatting in logging, i.e. not f-strings

~~~
>>> name = 'John'
>>> print(f'Hello {name}')
Hello John

~~~


~~~
>>> logging.warning('%s is here', name)
WARNING:root:John is here

~~~

---

### Have multiple loggers

* work with logging objects
* each with its own name
* own configuration

~~~
>>> mylogger = logging.getLogger('mylogger')
>>> mylogger.warning('%s is here', name)
WARNING:mylogger:John is here

~~~

write to file

~~~
>>> logging.basicConfig(level=logging.INFO, filename='log.txt', force=True)
>>> mylogger = logging.getLogger('mylogger')
>>> mylogger.info('%s is here', name)

~~~

    #log.txt
    INFO:mylogger:John is here
