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
