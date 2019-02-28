# How to upgrade python version

This is one way of compiling Python code and have it working on your system (Debian based)

Always visit [Download Python | Python.org](https://www.python.org/downloads/) to check the newest releases of Python.

```bash
#!/usr/bin/env bash

python_version=3.7.2

sudo apt-get update
sudo apt-get -y install \
  build-essential \
  checkinstall \
  libbz2-dev \
  libc6-dev \
  libffi-dev \
  libgdbm-dev \
  libncursesw5-dev \
  libreadline-gplv2-dev \
  libsqlite3-dev \
  libssl-dev \
  tk-dev

cd /usr/src
sudo wget https://www.python.org/ftp/python/${python_version}/Python-${python_version}.tgz
sudo tar xzf Python-${python_version}.tgz
cd Python-${python_version}
sudo ./configure --enable-optimizations
sudo make -j 4 altinstall

cd ~/

# Just being sure that the new version is installed
python3.7 -V

# If we check the current alternatives it will break
# update-alternatives --list python3

# One can always add other alternatives
sudo update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python3.7 1

# Just showing off that things are working now
namei "$(which python3)"
python3 -V
/usr/bin/env python3 -V
```

After executing this process, `pip` failed because it could not find `lsb_release`, creating a symbolic link solved this issue:

```bash
sudo ln -s /usr/share/pyshared/lsb_release.py /usr/local/lib/python3.7/site-packages/lsb_release.py
```

Possible with the help of:

- [Python 3.6 - install latest version into Linux Mint](https://mintguide.org/other/794-python-3-6-install-latest-version-into-linux-mint.html)
- [How To Install Python 3.7.0 on Ubuntu, Debian & LinuxMint - TecAdmin](https://tecadmin.net/install-python-3-7-on-ubuntu-linuxmint/)
- [python3 - No module named 'lsb_release' after install Python 3.6.3 from source - Ask Ubuntu](https://askubuntu.com/questions/965043/no-module-named-lsb-release-after-install-python-3-6-3-from-source#answer-1003535)
