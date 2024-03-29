# Hints, tips and tricks in python

# Creating a simple webserver

To create a simple webserver:

**Python 2:**

    python -m SimpleHTTPServer 7777

**Python 3:**

    python -m http.server 7777

# Vendo os atributos de um objeto

Seguindo a [dica do StackOverflow](http://stackoverflow.com/questions/25150955/python-iterating-through-object-attributes) conseguimos ver os atributos e respectivos valores de qualquer obejto em python:

    for attr, value in objeto.__dict__.iteritems():
        print attr, value

# Solving AttributeError: 'module' object has no attribute 'packaging'

To the "AttributeError: 'module' object has no attribute 'packaging'" error I did as stated [here](http://stackoverflow.com/questions/28967785/attribute-error-installing-with-pip/30310206#30310206):

1. Start a python session, import pkg_resources, and view the file from which it is loaded:

    In [1]: import pkg_resources

    In [2]: pkg_resources.__file__
    Out[2]: '/usr/lib/python2.7/dist-packages/pkg_resources.pyc'

2. Remove this file (and the associated *.py file):

    $ sudo rm /usr/lib/python2.7/dist-packages/pkg_resources.py*

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

# Working with VirtualEnv

Withe the help of [Virtual Environments — The Hitchhiker's Guide to Python](http://docs.python-guide.org/en/latest/dev/virtualenvs/):

Install virtualenv first with:

```
sudo pip install virtualenv
```

Then create and use a virtual environment for the project:

```
cd project
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
# Do what you have to do
deactivate
```

