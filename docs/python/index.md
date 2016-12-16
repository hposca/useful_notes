# Hints, tips and tricks in python

## Locating the line number of an exception

From this [answer in StackOverflow](http://stackoverflow.com/questions/6961750/locating-the-line-number-where-an-exception-occurs-in-python-code#answer-6961861):

```
import traceback
import sys

try:
    raise Exception("foo")
except:
    for frame in traceback.extract_tb(sys.exc_info()[2]):
        fname,lineno,fn,text = frame
        print "Error in %s on line %d" % (fname, lineno)
```
