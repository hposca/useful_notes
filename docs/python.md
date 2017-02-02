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
