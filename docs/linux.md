
<!-- vim-markdown-toc GFM -->

* [Print n-th column to last column](#print-n-th-column-to-last-column)
    * [Use cases](#use-cases)
* [Wiping a file](#wiping-a-file)
* [How to use multiple dropbox accounts on the same computer](#how-to-use-multiple-dropbox-accounts-on-the-same-computer)
* [Find and Replace string in all files](#find-and-replace-string-in-all-files)
* [How to convert between formats](#how-to-convert-between-formats)
    * [m4a to mp3](#m4a-to-mp3)
        * [Automatically](#automatically)
* [How to remove accent characters](#how-to-remove-accent-characters)
* [Finding Hardware Information on Linux](#finding-hardware-information-on-linux)
* [Hibernation Problems](#hibernation-problems)
* [Merge/Join images on command line](#mergejoin-images-on-command-line)
* [Dicas Linux](#dicas-linux)
    * [How do I list all cron jobs for all users?](#how-do-i-list-all-cron-jobs-for-all-users)
    * [How to create a CPU spike with a bash command](#how-to-create-a-cpu-spike-with-a-bash-command)
* [Rsync](#rsync)
    * [Pegando e enviando arquivos para a Amazon](#pegando-e-enviando-arquivos-para-a-amazon)
    * [Apenas arquivos novos](#apenas-arquivos-novos)
    * [How to gzip a directory, transfer via scp, and decompress in one command?](#how-to-gzip-a-directory-transfer-via-scp-and-decompress-in-one-command)
    * [Rsync on a different SSH port](#rsync-on-a-different-ssh-port)
* [Encontrando arquivos com setuid ativado](#encontrando-arquivos-com-setuid-ativado)
* [Mouse](#mouse)
* [Sort](#sort)
        * [Sort com delimitador diferente de espaços](#sort-com-delimitador-diferente-de-espaços)
        * [Sort n-ésima coluna](#sort-n-ésima-coluna)
        * [Sort com ordenação numérica de verdade ( 9 < 100 )](#sort-com-ordenação-numérica-de-verdade--9--100-)
    * [Ativando swap](#ativando-swap)
* [Loop com arquivos que têm espaços no nome](#loop-com-arquivos-que-têm-espaços-no-nome)
* [Descobrindo a placa de vídeo](#descobrindo-a-placa-de-vídeo)
* [Copying](#copying)
    * [Avoid copy if files exist in destination](#avoid-copy-if-files-exist-in-destination)
* [Displaying Today’s Files Only](#displaying-todays-files-only)
* [How to zip files that are listed in a text file](#how-to-zip-files-that-are-listed-in-a-text-file)
* [Desabilitando um usuário, mas sem apagá-lo](#desabilitando-um-usuário-mas-sem-apagá-lo)
* [IPTables (Firewall)](#iptables-firewall)
* [How to run multiple simultaneous X Window sessions](#how-to-run-multiple-simultaneous-x-window-sessions)
* [Create another SSH server](#create-another-ssh-server)
* [No wired connection after suspend](#no-wired-connection-after-suspend)
* [Validating RSA public keys](#validating-rsa-public-keys)
* [Getting you own key from the command line](#getting-you-own-key-from-the-command-line)
* [Getting home directory by username](#getting-home-directory-by-username)
* [How do I find out which process is eating up my bandwidth](#how-do-i-find-out-which-process-is-eating-up-my-bandwidth)
* [Discovering available versions of a package to install](#discovering-available-versions-of-a-package-to-install)
* [Copying text from the command line](#copying-text-from-the-command-line)
* [Which package an executable came from?](#which-package-an-executable-came-from)
* [Finding the size of files older than X days](#finding-the-size-of-files-older-than-x-days)
* [Create disk image](#create-disk-image)
* [Restore system](#restore-system)
* [How to create a temporary ftp server](#how-to-create-a-temporary-ftp-server)
* [Group AWS ECR images](#group-aws-ecr-images)
* [Onde logs estão localizados no Linux](#onde-logs-estão-localizados-no-linux)
    * [Logs de ssh](#logs-de-ssh)
* [Simple monitoring with gkrellm](#simple-monitoring-with-gkrellm)
* [Limiting the bandwith for a program](#limiting-the-bandwith-for-a-program)
* [Discover other machines on your network](#discover-other-machines-on-your-network)
* [Updating the DNS being used](#updating-the-dns-being-used)
* [Installing](#installing)
* [Before using it](#before-using-it)
    * [Exporting Keys](#exporting-keys)
* [Setting up](#setting-up)
* [Reaffirming trust](#reaffirming-trust)
* [Reduce size of PDF files](#reduce-size-of-pdf-files)
* [Splitting and merging PDF files](#splitting-and-merging-pdf-files)
    * [Splitting](#splitting)
    * [Merging](#merging)
* [Joining/merging pdf pages/files into a single one](#joiningmerging-pdf-pagesfiles-into-a-single-one)
* [Join png files into single PDF](#join-png-files-into-single-pdf)
* [Convert PDFs into JPEGs](#convert-pdfs-into-jpegs)
    * [All pages into the same file](#all-pages-into-the-same-file)
* [Printers Adventures on Linux](#printers-adventures-on-linux)
* [Displaying a function definition](#displaying-a-function-definition)
* [Bang commands](#bang-commands)
    * [Useful idioms](#useful-idioms)
    * [Retrieving previous command arguments](#retrieving-previous-command-arguments)
    * [Lines in history](#lines-in-history)
    * [Word designators](#word-designators)
    * [Modifiers:](#modifiers)
* [Send command to all panes](#send-command-to-all-panes)
* [How to upgrade python version](#how-to-upgrade-python-version)
* [Wifi after hibernation](#wifi-after-hibernation)
* [Download a video from youtube](#download-a-video-from-youtube)
* [Convert the downloaded audio](#convert-the-downloaded-audio)
* [Create a ramdisk](#create-a-ramdisk)
* [Get your current IPs](#get-your-current-ips)
* [Resetting the gpg-agent](#resetting-the-gpg-agent)
* [VirtualBox shared folder](#virtualbox-shared-folder)

<!-- vim-markdown-toc -->

# Print n-th column to last column

From [this StackOverflow thread](http://stackoverflow.com/questions/1602035/print-third-column-to-last-column), this is how you can print from the n-th column to the last column of a line, using awk.

e.g. form the third column:

    awk '{ print substr($0, index($0,$3)) }'

## Use cases

This is useful with you want to analyse your own command's history, like:

    history | awk '{print substr($0, index($0,$2))}'

# Wiping a file

```bash
#!/usr/bin/env bash
set -euo pipefail
IFS=$'\n\t'

if [[ "$#" -ne 1 ]]; then
  echo "Please use this command with the filename you want to wipe. Like:"
  echo "$0 file.txt"
  exit 1
fi

_file="${1}"

_times=8
filesize=$(stat -c %s "${_file}")

# Zeroes
dd if=/dev/zero of="${_file}" bs="${filesize}" count=1 conv=notrunc
# Ones
dd if=<(yes $'\xFF' | tr -d '\n') of="${_file}" bs="${filesize}" count=1 conv=notrunc

# Random stuff
for (( i = 0; i < _times; i++ )); do
  dd if=/dev/urandom of="${_file}" bs="${filesize}" count=1 conv=notrunc
done
```

Made possible with the help of [Securely Wipe a File with DD](https://www.marksanborn.net/security/securely-wipe-a-file-with-dd/) and [How do I get an equivalent of /dev/one in Linux](https://stackoverflow.com/questions/10905062/how-do-i-get-an-equivalent-of-dev-one-in-linux)

# How to use multiple dropbox accounts on the same computer

From [How Can I Use Multiple Dropbox Accounts on the Same Computer? - The Unofficial Dropbox Wiki](http://www.dropboxwiki.com/forums-faq/running-multiple-instances-of-dropbox):

---

First, create an alternate (hidden) directory for your second Dropbox account:

    mkdir ~/.dropbox-alt

Then run the Dropbox installer in “first use” mode, specifying the alternate home directory as follows:

    HOME=~/.dropbox-alt dropbox start -i

To run the “alternate” Dropbox manually for testing:

    HOME=~/.dropbox-alt ~/.dropbox-alt/.dropbox-dist/dropboxd

Create a shell script to start the alternate Dropbox daemon:

    cd ~/.dropbox-alt           # change to the "alternate" Dropbox directory
                                # which was created above
    gedit dropbox-alt.sh        # create a new file with the 'gedit' text editor

Copy and paste the following lines into the editor:

    #!/bin/bash
    HOME=$HOME/.dropbox-alt     # set the new home dir
    cd                          # to the "new" $HOME
    ./.dropbox-dist/dropboxd &  # and run dropboxd

Set the dropbox-alt.sh script to auto-start on login!

# Find and Replace string in all files

From [Find and Replace string in all files recursive using grep and sed](http://stackoverflow.com/questions/15920276/find-and-replace-string-in-all-files-recursive-using-grep-and-sed):

    grep -rl $oldstring /path/to/folder | xargs sed -i s@$oldstring@$newstring@g

# How to convert between formats

## m4a to mp3

    ffmpeg -i <file>.m4a -acodec libmp3lame -ab 128k <file>.mp3

### Automatically

    for file in *m4a; do echo ffmpeg -i $file -acodec libmp3lame -ab 128k ${file%.*}.mp3; done

# How to remove accent characters

With the help of: http://www.linuxquestions.org/questions/linux-newbie-8/how-to-remove-accent-characters-4175431373/

    find . -type f -iname "*.py" | while read file; do
        iconv -f UTF-8 -t ASCII//TRANSLIT $file > $file-tmp
        mv $file-tmp $file
    done

# Finding Hardware Information on Linux

From [How to find information about hardware? - Linux Mint Forums](https://forums.linuxmint.com/viewtopic.php?t=136772)

A more human readable output:

```
inxi -Fx
```

This one was super useful in discovering the name of the Motherboard I was using.

For more detailed info:

```
lshw
```

# Hibernation Problems

After installing Linux Mint 20 Hibernation didn't work.

To make it work I followed [this post](https://forums.linuxmint.com/viewtopic.php?p=1495203&sid=9b5eeffffe5ffd2073e04adbb276d15e#p1495203) on how to update grub and merging [this post's](https://forums.linuxmint.com/viewtopic.php?p=1854165#p1854165) `Action`s of the `com.ubuntu.enable-hibernate.pkla` file.

And creating the service to disable devices to wake up from hibernation as this [SO guide](https://unix.stackexchange.com/questions/602406/guide-on-how-to-enable-hibernation-on-linux-mint-20-cinnamon-ubuntu-20-and-pre), in my case I wanted to disable the keyboard from waking up the computer.

# Merge/Join images on command line

Requires ImageMagick:

Vertical sprite:

`convert image1.png image2.png image3.png -append result/result-sprite.png`

Horizontal sprite:

`convert image1.png image2.png image3.png +append result/result-sprite.png`

﻿
# Dicas Linux

## How do I list all cron jobs for all users?

From [How do I list all cron jobs for all users?](http://stackoverflow.com/questions/134906/how-do-i-list-all-cron-jobs-for-all-users) :

~~~ Bash
# As root:
for user in $(cut -f1 -d: /etc/passwd); do echo $user; crontab -u $user -l; done
~~~

## How to create a CPU spike with a bash command

From [How to create a CPU spike with a bash command](http://stackoverflow.com/questions/2925606/how-to-create-a-cpu-spike-with-a-bash-command) :

~~~ Bash
dd if=/dev/zero of=/dev/null
~~~

To run more of those to put load on more cores, try to fork it:

~~~ Bash
fulload() { dd if=/dev/zero of=/dev/null | dd if=/dev/zero of=/dev/null | dd if=/dev/zero of=/dev/null | dd if=/dev/zero of=/dev/null & }; fulload; read; killall dd
~~~

Repeat the command in the curly brackets as many times as the number of threads you want to produce (here 4 threads). Simple enter hit will stop it (just make sure no other dd is running on this user or you kill it too).

# Rsync

## Pegando e enviando arquivos para a Amazon

~~~ Bash
rsync -avz --progress -e "ssh -i chave.pem" ubuntu@ip:arquivo .
~~~

## Apenas arquivos novos

Dicas [aqui](http://www.electrictoolbox.com/rsync-ignore-existing-update-newer/)

~~~ Bash
rsync --update -raz --progress <origem> <destino>
~~~

## How to gzip a directory, transfer via scp, and decompress in one command?

~~~ Bash
rsync -az --progress source_dir/* remote_host:/destination_dir # Show progress bar
#or:
tar -zc path/to/source | ssh user@remote tar -zxC path/to/destination # Doesn't show progress bar
~~~

## Rsync on a different SSH port

From [Is it possible to specify a different ssh port when using rsync? - Stack Overflow](https://stackoverflow.com/questions/4549945/is-it-possible-to-specify-a-different-ssh-port-when-using-rsync):

`rsync -azvP -e 'ssh -p 2222' ./dir user@host:/path`

# Encontrando arquivos com setuid ativado

Com [ajuda](http://www.cyberciti.biz/faq/unix-bsd-linux-setuid-file/), conseguimos encontrar todos os arquivos que estão com setuid ativado. Isso foi útil para resolver um problema que estava dando no SAS devido ao fato dos bin não estarem setados com ele:

~~~ Bash
find . -xdev \( -perm -4000 \) -type f -print0 | xargs -0 ls -l
~~~

Para se setar o setuid basta se adicionar qual bit será será setado como steuid (u,g,o):

~~~ Bash
chmod 4444 arquivo
# ou
chmod 2444 arquivo
# ou
chmod 1444 arquivo
~~~

# Mouse

Quando o touchpad parar de funcionar basta executar estes dois comandos:

~~~ Bash
# Como root:
modprobe -r psmouse
modprobe psmouse
~~~

# Sort

### Sort com delimitador diferente de espaços

~~~ Bash
sort -t: myFile
~~~

### Sort n-ésima coluna

~~~ Bash
sort -k3,3 myFile
~~~

### Sort com ordenação numérica de verdade ( 9 < 100 )

~~~ Bash
sort -n myFile
~~~

## Ativando swap

Ativando swap, seguindo o [tutorial da DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04):

~~~
#!/usr/bin/env bash
if [ $(id -u) -ne 0 ]; then echo "Run as root"; exit 1; fi
free -m
fallocate -l 4G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
swapon -s
free -m
~~~

# Loop com arquivos que têm espaços no nome

Dicas extraídas [daqui](http://askubuntu.com/questions/343727/filenames-with-spaces-breaking-for-loop-find-command) e [daqui](http://www.cyberciti.biz/tips/handling-filenames-with-spaces-in-bash.html).

~~~ Bash
find . -print0 | while read -d $'\0' file; do cp -v "$file" /tmp; done
~~~

~~~ Bash
find . -type f -name '*.*' -printf '%p\0' | tar --null -uf archive.tar -T -
~~~

# Descobrindo a placa de vídeo

Para se decobrir qual placa de vídeo estamos usando podemos utilizar o `lspci` como descrito [aqui](http://www.binarytides.com/linux-get-gpu-information/) e [aqui](http://www.cyberciti.biz/faq/linux-tell-which-graphics-vga-card-installed/):

~~~ Bash
lspci -v
~~~

# Copying

## Avoid copy if files exist in destination

    cp -R -u -p /source /destination

Due to these options:

    -p                           same as --preserve=mode,ownership,timestamps
        --preserve[=ATTR_LIST]   preserve the specified attributes (default:
                                mode,ownership,timestamps), if possible
                                additional attributes: context, links, xattr,
                                all
    -R, -r, --recursive          copy directories recursively
        --reflink[=WHEN]         control clone/CoW copies. See below
        --remove-destination     remove each existing destination file before
                                attempting to open it (contrast with --force)
        --sparse=WHEN            control creation of sparse files. See below
        --strip-trailing-slashes  remove any trailing slashes from each SOURCE
                                argument
    -u, --update                 copy only when the SOURCE file is newer
                                than the destination file or when the
                                destination file is missing

# Displaying Today’s Files Only

    find -maxdepth 1 -type f -mtime -1

# How to zip files that are listed in a text file

    cat input.txt | zip filename.zip -@

# Desabilitando um usuário, mas sem apagá-lo

    usermod --expiredate 1 [LOGIN]

    passwd -l [LOGIN]

# IPTables (Firewall)

Algumas dicas interessantes:

- [Apagando uma regra do firewall por linha](http://stackoverflow.com/questions/10197405/iptables-remove-specific-rules)
- [Um bom tutorial explicando como funciona](http://www.thegeekstuff.com/2011/02/iptables-add-rule/)
- [Um tutorial simples](https://wiki.centos.org/HowTos/Network/IPTables)

Listando as regras, com portas e número das linhas:

    iptables -L -n --line-numbers

Apagando uma linha específica:

    iptables -D INPUT [num]

Adicionando regra para novas conexões:

    iptables -A INPUT -m state --state NEW -p tcp --dport [porta] -j ACCEPT

Regra para se rejeitar demais conexões:

    iptables -A INPUT -j REJECT --reject-with icmp-host-prohibited
    iptables -A INPUT -j DROP

Bloqueando acesso de saída (upload) de um IP:

    iptables -A OUTPUT -d [ip-address] -j DROP

Bloqueando saída por porta:

    iptables -A OUTPUT -p tcp --dport [port] -j DROP

To block tcp port for an IP address:

    iptables -A OUTPUT -p tcp -d [ip address] --dport [port] -j DROP

# How to run multiple simultaneous X Window sessions

From [Linux Commando](http://linuxcommando.blogspot.com.br/2015/02/how-to-run-multiple-simultaneous-x.html)

- Switch to a virtual terminal
    - By default, six virtual text terminals are available to you (Ctrl+Alt+F1 to F6)
    - Press Control+Alt+F1 to go to virtual terminal 1
- Login as her
- Execute the following command
    `startx -- :1`
- This starts another X session using the first free graphical console.
    - By default, 6 X consoles are available (Ctrl+Alt+F7 to F12)
    - Your own existing X session is Ctrl+Alt+F7
    - The next free X console is therefore Ctrl+Alt+F8
- To switch from her X session to yours, and vice versa, press Ctrl+Alt+F7 and Ctrl+Alt+F8 respectively.

You may use the above procedure to create up to 5 additional X sessions (Ctrl+Alt+F8 to Ctrl+Alt+F12).
For each additional X session, increment the console number in both step 1 and 3.
For instance, switch to virtual terminal 2 (Control+Alt+F2) to execute the command `startx -- :2`.

# Create another SSH server

Sometimes you need to debug your SSH connection, for those times you can open another SSH server and watch what is happening inside it.

    /usr/sbin/sshd -d -p 2222

And when (from other machine) need to connect to it:

    ssh -p 2222 user@machine

# No wired connection after suspend

**(Not tested yet)**

From [this Ubuntu Forum thread](http://ubuntuforums.org/showthread.php?t=2179603):

- Get the Kernel module with `lspci -k | grep -iA3 NETWORK` or `lspci -k | grep -iA3 ethernet`
- Edit `/etc/pm/config.d/modules`
    - Add `SUSPEND_MODULES="<the_kernel_module_for_your_network_card>"`

Or in one shot:

    lspci -k | grep -iA3 ethernet | grep -i "Kernel driver" | awk '{print $NF}'

Knowing that your Kernel module is 8139:

1. Unload 8139:

    sudo modprobe -r 8139

2. Suspend:

    sudo pm-suspend

3. Resume the computer from suspend
4. Reload the module:

    sudo modprobe 8139

5. Test the network connection.

# Validating RSA public keys

    ssh-keygen -l -f key.pub

More info on [How do I validate a RSA ssh public key file (id_rsa.pub)? - Server Fault](http://serverfault.com/questions/453296/how-do-i-validate-a-rsa-ssh-public-key-file-id-rsa-pub)

# Getting you own key from the command line

To get your fingerprint

    ssh-add -l

To get your public key

    ssh-add -L

This is useful when you are inside a Docker container that has your keys forwarded to it using:

    -v $(dirname $SSH_AUTH_SOCK):$(dirname $SSH_AUTH_SOCK)
    -e SSH_AUTH_SOCK=$SSH_AUTH_SOCK

# Getting home directory by username

    eval echo ~$USER

# How do I find out which process is eating up my bandwidth

From [networking - How do I find out which process is eating up my bandwidth? - Ask Ubuntu](http://askubuntu.com/questions/2411/how-do-i-find-out-which-process-is-eating-up-my-bandwidth):

    sudo apt-get install -y nethogs
    sudo nethogs eth0

Another way:

- Run `sudo iftop`
- Push `S` or `D` to display Source and Destination ports
- Then use `netstat -tup` to discover the process

# Discovering available versions of a package to install

Discover which versions are available using one of the following commands:

    apt-cache policy <package-name>
    apt-cache madison <package-name>

Then, to install the chosen version:

    sudo apt-get install package=version

# Copying text from the command line

    xclip -sel clip < ~/.ssh/id_rsa.pub

# Which package an executable came from?

With the help of [apt - How can I tell which package an executable came from? - Ask Ubuntu](http://askubuntu.com/questions/257905/how-can-i-tell-which-package-an-executable-came-from) there are two alternatives:

```
sudo apt-get install apt-file
# It takes a bit of time
sudo apt-file update
apt-file search filename
```

OR

```
dpkg-query -S [executable]
```

# Finding the size of files older than X days

With the great help of [StackOverflow](http://unix.stackexchange.com/questions/21678/is-there-a-way-to-sum-up-the-size-of-files-listed):

```
find . -mtime +180 -exec ls -lrt {} \; | awk '{ total += $5}; END { print total }'
```

It will give the total size of files, in bytes.

But if you want to see the sizes in a more human readable way:

```
find . -mtime +180 -exec ls -lrt {} \; | awk '{ total += $5}; END { print total }' | awk '{ split( "KB MB GB" , v ); s=0; while( $1>1024 ){ $1/=1024; s++ } print int($1) v[s] }'
```

# Create disk image

From [dd - ArchWiki](about:reader?url=https%3A%2F%2Fwiki.archlinux.org%2Findex.php%2FDd%23Disk_cloning_and_restore#Disk_cloning_and_restore)

Boot from a live media and make sure no partitions are mounted from the source hard drive.

Then mount the external hard drive and backup the drive:

`dd if=/dev/sda conv=sync,noerror bs=64K | gzip -c  > /path/to/backup.img.gz`

If necessary (e.g. when the format of the external HD is FAT32) split the disk image in volumes (see also the split man pages):

`dd if=/dev/sda conv=sync,noerror bs=64K | gzip -c | split -a3 -b2G - /path/to/backup.img.gz`

If there is not enough disk space locally, you may send the image through ssh:

`dd if=/dev/sda conv=sync,noerror bs=64K | gzip -c | ssh user@local dd of=backup.img.gz`

Finally, save extra information about the drive geometry necessary in order to interpret the partition table stored within the image. The most important of which is the cylinder size.

`fdisk -l /dev/sda > /path/to/list_fdisk.info`

Note: You may wish to use a block size (bs=) that is equal to the amount of cache on the HD you are backing up. For example, bs=8192K works for an 8 MiB cache. The 64 KiB mentioned in this article is better than the default bs=512 bytes, but it will run faster with a larger bs=.

# Restore system

To restore your system:

`gunzip -c /path/to/backup.img.gz | dd of=/dev/sda`

When the image has been split, use the following instead:

`cat /path/to/backup.img.gz* | gunzip -c | dd of=/dev/sda`

# How to create a temporary ftp server

Using [pyftpdlib](https://pyftpdlib.readthedocs.io/en/latest/tutorial.html#command-line-usage):

```
pip3 install --user pyftpdlib
python3 -m pyftpdlib --port=2121 --write --username=user --password=password
```

This will create an FTP server listening on port `2121` accepting connections that can write into the current directory.

# Group AWS ECR images

With the help of https://github.com/stedolan/jq/issues/274#issuecomment-64925860

```bash
aws ecr list-images --repository-name ${repository_name} | jq '.[] | group_by(.imageDigest) | map({"digest": .[0].imageDigest, "tags": map(.imageTag)})'
```

This will display all the tags associated with the image digest.

[xorg - List all valid kbd layouts, variants and toggle options (to use with setxkbmap)](https://unix.stackexchange.com/a/298947):

        Take a look at localectl, especially following options:

        localectl list-x11-keymap-layouts - gives you layouts (~100 on modern systems)
        localectl list-x11-keymap-variants de - gives you variants for this layout (or all variants if no layout specified, ~300 on modern systems)
        localectl list-x11-keymap-options | grep grp: - gives you all layout switching options

# Onde logs estão localizados no Linux

## Logs de ssh

- /var/log/auth.log
- /var/log/secure
- `journalctl -u sshd | tail -100`

# Simple monitoring with gkrellm

On the remote (to be monitored computer):

```bash
sudo sh -c 'apt-get update && apt-get install -y gkrellmd'
```

On your local machine, create an SSH tunnel to the remote machine:

```bash
ssh -N -f -L 19151:127.0.0.1:19150 user@remote_machine_ip
```

Open gkrellm on your machine listening to the local port that will receive information:

```
gkrellm -s 127.0.0.1 -P 19151
```

Help from https://totallynoob.com/server-monitoring-remote-monitor-a-server-with-gkrellm-via-ssh-tunnel/

# Limiting the bandwith for a program

```bash
trickle -s -d 100 -u 100 <program>
```

Uses `trickle` to limit the bandwith that will be used by a program.

# Discover other machines on your network

Grab your IP with a `ifconfig` then later use a `nmap` to scan your network, something like:

```bash
sudo nmap -sP 10.253.0.0/16
```

# Updating the DNS being used

Check which DNS you are using:

```bash
nslookup google.com
```

It is using the IPs that appear on `Server` and `Address`.
To change it:

Add

```
nameserver 8.8.8.8
nameserver 1.1.1.1
```

to `/etc/resolvconf/resolv.conf.d/head` and

```bash
sudo resolvconf -u
```

Validate that the DNS has been updated:

```bash
nslookup google.com
```

Thanks to https://www.linuxquestions.org/questions/linux-networking-3/how-to-change-the-dns-server-in-linux-mint-and-should-i-4175597949/#post5658301

# Installing

    sudo apt-get install -y pass

Do not forget to configure the [completion](https://git.zx2c4.com/password-store/tree/src/completion) for your preferred shell.

# Before using it

Before using `pass` we need to have a key generated by `GnuPG` with:

    gpg2 --gen-key

As it may take a lot of time, specially for it to gather enough entropy, one can use this solution from [StackOverflow](https://serverfault.com/questions/471412/gpg-gen-key-hangs-at-gaining-enough-entropy-on-centos-6):

> Anytime I've seen it (gpg --gen-key) hang like this, I log in to another shell and start a "dd if=/dev/sda of=/dev/zero" and it takes off after a few seconds / minutes.

By my experience, even using this method it still took around 10 minutes for the key generation.

## Exporting Keys

If you need to export your keys to another machine one can follow the commands that are described in this [StackOverflow question](https://unix.stackexchange.com/questions/226944/pass-and-gpg-no-public-key).

    $ gpg --export-secret-keys > keyfile
    $ gpg2 --import keyfile

Here they explicitly use different versions of GnuPG as the new versions of `pass` use `gpg2`.

# Setting up

According to the [official site tutorial](https://www.passwordstore.org/) and the [extended git example](https://git.zx2c4.com/password-store/about/#EXTENDED%20GIT%20EXAMPLE) we can start using it like this:

- List your current available keys

    gpg2 --list-key

- And use one of those keys with pass

    pass init key@mail

    pass git init

- All files will be saved on `~/.password-store` directory

- If you have somewhere to push your passwords

    pass git remote add origin the-place-you-will-push-to:branch

- Start inserting or generating your passwords

    pass generate A/B/mail@place.com 21
    pass insert A/B/mail@place.com

- When you are finished saving your passwords push them

    pass git push -u --all

More information on how to use `pass` can be found on [its page](https://www.passwordstore.org/) and reading the [man page](https://git.zx2c4.com/password-store/about/).

# Reaffirming trust

If you receive some error like this one after reimporting your key

```
gpg: <KEY>: There is no assurance this key belongs to the named user
gpg: [stdin]: encryption failed: Unusable public key
```

you can do as suggest on this [StackOverflow thread](https://stackoverflow.com/questions/33361068/gnupg-there-is-no-assurance-this-key-belongs-to-the-named-user) and do a

```bash
gpg2 --edit-key <KEY>
gpg> trust
# Answer that you trust ultimately (5)
# Answer yes
```

# Reduce size of PDF files

- [compression - How can I reduce the file size of a scanned PDF file? - Ask Ubuntu](http://askubuntu.com/questions/113544/how-can-i-reduce-the-file-size-of-a-scanned-pdf-file)
- [compression - How to reduce the size of a pdf file? - Ask Ubuntu](http://askubuntu.com/questions/207447/how-to-reduce-the-size-of-a-pdf-file)
- [Reducing PDF file-size in Linux | The Road to Elysium](http://jorge.fbarr.net/2012/11/29/reducing-pdf-file-size-in-linux/)
- [Bash Shell: Ignore Aliases and Functions When Running A Command](http://www.cyberciti.biz/faq/ignore-shell-aliases-functions-when-running-command/)

Low quality options:

    gs -dNOPAUSE -dBATCH -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/screen -sOutputFile=new_file.pdf original_file.pdf
    gs -dNOPAUSE -dBATCH -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook -sOutputFile=new_file.pdf original_file.pdf

The best balance was found in:

    gs -dNOPAUSE -dBATCH -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/default -sOutputFile=new_file.pdf original_file.pdf

**Note:** You may need to issue the command with a backslash in front of it if you use 'gs' as an alias:

    \gs -dNOPAUSE -dBATCH -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/default -sOutputFile=new_file.pdf original_file.pdf

# Splitting and merging PDF files

With hints from:

- [Linux Commando: Splitting up is easy for a PDF file](http://linuxcommando.blogspot.com.br/2013/02/splitting-up-is-easy-for-pdf-file.html)
- [Linux Commando: How to split up PDF files - part 2](http://linuxcommando.blogspot.com.br/2014/01/how-to-split-up-pdf-files-part-2.html)
- [Linux Commando: How to merge or split pdf files using convert](http://linuxcommando.blogspot.com.br/2015/03/how-to-merge-or-split-pdf-files-using.html)

    sudo apt-get update && sudo apt-get install -y pdftk

## Splitting

You can specify page ranges like this:

    pdftk myoldfile.pdf cat 1-2 4-5 output mynewfile.pdf

pdftk has a few more tricks in its back pocket. For example, you can specify a burst operation to split each page in the input file into a separate output file.

    pdftk myoldfile.pdf burst

By default, the output files are named pg_0001.pdf, pg_0002.pdf, etc.

## Merging

pdftk is also capable of merging multiple pdf files into one pdf.

    pdftk pg_0001.pdf pg_0002.pdf pg_0004.pdf pg_0005.pdf output mynewfile.pdf

That would merge the files corresponding to the first, second, fourth and fifth pages into a single output pdf.

# Joining/merging pdf pages/files into a single one

Install `pdfjam` from `texlive-latex-recommended`:

```bash
sudo apt-get install -y texlive-latex-recommended
```

Merge files with:

```bash
pdfjam Page1.pdf Page2.pdf --nup 2x1 --landscape --outfile Page1_2.pdf
```

Merge pages from the same file:

```bash
pdfjam file.pdf --nup 2x1 --landscape --outfile together.pdf
```

All this was possible with the help of https://superuser.com/questions/366490/how-to-merge-multiple-pdf-files-onto-one-page-with-pdftk/750293#750293

# Join png files into single PDF

```
pdfjoin --a4paper --fitpaper false --rotateoversize false *.png
```

Or

```
convert *.png -compress jpeg -quality 50 result.pdf
```

Thanks to: https://stackoverflow.com/questions/4778635/merging-png-images-into-one-pdf-file

# Convert PDFs into JPEGs

## All pages into the same file

```
for i in *.pdf; do convert -density 600 -append $i ${i%.*}.jpg; done
```

With the help of [linux - How to merge images in command line? - Stack Overflow](https://stackoverflow.com/questions/20075087/how-to-merge-images-in-command-line)

# Printers Adventures on Linux

To make the Brother MFC-L2730DW work (I was only able to make it work on a LinuxMint 18.3 Virtual Machine, couldn't make it work on 20)

Basically you have to [download the drivers](https://support.brother.com/g/b/downloadlist.aspx?c=ca&lang=en&prod=mfcl2730dw_us_eu_as&os=128) only the "Driver install tool is enough" and follow the recommendations from [How to Make a Brother Printer and Scanner Work in Ubuntu](https://wayneoutthere.com/2016/05/28/brother-printer-scanner-ubuntu-how-to/) and update the `/lib/udev/rules.d/60-libsane1.rules` file with:

```
ATTRS{idVendor}=="04f9", ATTRS{idProduct}=="0439", ENV{libsane_matched}="yes"
```

---

Copy Paste of [How to Make a Brother Printer and Scanner Work in Ubuntu](https://wayneoutthere.com/2016/05/28/brother-printer-scanner-ubuntu-how-to/):

```
EDIT 2020-06-19
Long time no edit!  My instructions below still mostly work it seems (which is bad on Brother for not making this more simple – fail, fail fail)

I wanted to add a quick note for 18.04 generation Ubuntu friends to consider as you are working through my nasty old messy post below:

First, if you are installing a network scanner / printer and you get the question “Select the number of destination Device URI” question, I just type the number 14 (A): Auto. option at the bottom.  Seems to work…

Then, be ready to provide your printers IP address.  If you didn’t know you can usually grab this from your router admin settings (I find this easier) or you can find it by pushing some buttons on the printer itself but I recall this was annoying… in either case you will need to punch in the ip address of the printer during setup so have it ready.

For the ‘libsane udev’ stuff below, the file has seemingly changed in recent ubuntu.  It now seems to be

60-libsane1.rules

Hope these updates help!

———————————–

*THIS WILL UNDERGO SOME EDITS BETWEEN OCT 31st and Nov 4th, 2016.  If you can get some answers below, great, but hopefully next week it will be more clear and helpful to more models of printers.

*Make sure to read my edits below this before starting as some things have changed…

*many edits below!  don’t start till you’ve skimmed them all

*PRE-note: if you can buy HP it’s probably better for you.  If you like pain like me, or already have pain, read on.

For some reason Brother printers are kind of hard to make work in Ubuntu for me.  Especially the scanner part.  They claim to support ‘linux’ but it’s not typically plug in play for me.  However, they are ghetto cheap so I buy them and pay for the savings in set up pain.  Oh well.  But this time I’m wising up and I’m blogging this for myself (and mom) so that we can get it set up quicker when we do upgrades or machine changes.  The main issue always seems to be this:

    Install the drivers with the command line as per the ‘pretty decent’ generic software from Brother found here: LINK TO UBUNTU BROTHER PRINTER DRIVERS
    select ‘linux’select ‘Linux (deb’)’Choose ‘driver install tool’ if you can which gets both the printer and the scanner going.’Agree to the EULA and Download’

    save file.  it will go to your ‘downloads’ folder if you have not told your browser to download it somewhere else.  You will need to know this for the next part so take a moment after the download to confirm it downloaded and you know where it is.
    follow instructions that appear on brother site right after downloading drivers, but here they are as of today (make sure on their site it’s up to date and don’t fully trust mine).Step1. Download the tool.(linux-brprinter-installer-*.*.*-*.gz)The tool will be downloaded into the default “Download” directory.
    (The directory location varies depending on your Linux distribution.)
    e.g. /home/(LoginName)/Download

    Step2. Open a terminal window and go to the directory you downloaded the file to in the last step.

    Step3. Enter this command to extract the downloaded file:

    Command: gunzip linux-brprinter-installer-*.*.*-*.gz

    Step4. Get superuser authorization with the “su” command or “sudo su” command.

    Step5. Run the tool:

    Command: bash linux-brprinter-installer-*.*.*-* Brother machine name

    Step6. The driver installation will start. Follow the installation screen directions.


     When you see the message “Will you specify the DeviceURI ?”,

     For USB Users: Choose N(No)
     For Network Users: Choose Y(Yes) and DeviceURI.

    The install process may take some time. Please wait until it is complete.
    Do this:
    1. Open “/lib/udev/rules.d/40-libsane.rules” file with ‘sudo nano’ command
    2.  Add the following two lines to the end of the device list. (Before the line “# The following rule will disable …”):
    Copy to your computer memory this:
    # Brother scanners
    ATTRS{idVendor}==”04f9″, ENV{libsane_matched}=”yes”   <–Paste it in with the special ‘control+shift+v’ (don’t use just regular control+v) feature in terminal

    Reboot the machine (you can just type sudo reboot if you are still in terminal and want it done fast…)
    open simple scan software from dash and try a test scan

For me, without doing step #2 above the printer will usually work but not the scanner.

Which makes me wonder if there is really any Ubuntu support at all…

But my ghetto printer/scanner is doing its job so oh well.

Hope this helps!
```

- [Downloads | MFC-L2730DW | Canada | Brother](https://support.brother.com/g/b/downloadlist.aspx?c=ca&lang=en&prod=mfcl2730dw_us_eu_as&os=128)
- [How to Make a Brother Printer and Scanner Work in Ubuntu – Wayne Out There](https://wayneoutthere.com/2016/05/28/brother-printer-scanner-ubuntu-how-to/)

- [[SOLVED] Brother MFC-L2730DW - Scanner Segfault (24-bit Mode Only) - Linux Mint Forums](https://forums.linuxmint.com/viewtopic.php?t=262330)
- [printing - How do I diagnose Brother MFC scanner issues on Ubuntu Linux? - Ask Ubuntu](https://askubuntu.com/questions/911941/how-do-i-diagnose-brother-mfc-scanner-issues-on-ubuntu-linux)
- [Utilities | Downloads | MFC-L2730DW | Canada | Brother](https://support.brother.com/g/b/downloadhowto.aspx?c=ca&lang=en&prod=mfcl2730dw_us_eu_as&os=128&dlid=dlf006893_000&flang=4&type3=625)
- [Printer Brother MFC-L2730DW / MFC-L2732DW Driver Linux Mint 19 How to Download and Install | tutorialforlinux.com](https://tutorialforlinux.com/2018/12/20/printer-brother-mfc-l2730dw-mfc-l2732dw-driver-linux-mint-19-how-to-download-and-install/)
- [MFC-L2730DW FAQ Categories | Brother Support](https://www.brother.is/support/mfc-l2730dw/faqs/how-to-trouble-shooting)
- [CUPS/Printer-specific problems - ArchWiki](https://wiki.archlinux.org/index.php/CUPS/Printer-specific_problems)

- [virtualbox - How Can I connect USB Printer in Virtual Box OSE Win XP - Ask Ubuntu](https://askubuntu.com/questions/48982/how-can-i-connect-usb-printer-in-virtual-box-ose-win-xp)
- [How to enable USB in VirtualBox - TechRepublic](https://www.techrepublic.com/article/how-to-enable-usb-in-virtualbox/)
- [Downloads – Oracle VM VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Download_Old_Builds_6_1 – Oracle VM VirtualBox](https://www.virtualbox.org/wiki/Download_Old_Builds_6_1)

# Displaying a function definition

If you have a bash defined funcion, e.g. foobar, and want to display its definition you can:

`type foobar` To discover its location
`declare -f foobar` To display its definition

Found this info at [Can bash show a function's definition? - Stack Overflow](https://stackoverflow.com/questions/6916856/can-bash-show-a-functions-definition).

# Bang commands

||Description|
| -- | -- |
| !! | Will execute the last command. Same as !-1 or "up-arrow + return". The most common use is sudo !! when issued command failed due to insufficient privileges. |
| !n | Will execute the line n of the history record.   In most cases using mouse is a better deal |
!-n | Will execute the command n lines back.  You usually remember the last three-four command you issues so !-2 and !-3 speed things up. |
| !gzip | will re-execute the most recent gzip command (history is searched  in reverse order). String specified is considered to be the prefix of the  command. |
| !?etc.gz | same as above but the unique string doesn't have to be at the start of the command. That means that you can identify command by a unique string appearing anywhere in the command  (might be faster then Ctrl-R  mechanism) |
| !!:1 | designates the first argument of the last command.  This can be shortened to !1. |
| !!:$ | designates the last argument of the preceding command. This may be shortened to !$. |


I think you need you need to know  five major bang commands (!!, !!:h, !!:f, !$, !1)

## Useful idioms

||Description|
| -- | -- |
| sudo !! | reexecute the last command with prefix sudo. |
| cd !$ | cd to the directory which is the last argument of the previous command. |


## Retrieving previous command arguments

||Description|
| -- | -- |
| !:0 | is the previous command name |
| !^, !:2, !:3, …, !$ | are the arguments of the previous command |
| !* | is all the arguments |
| !$ | is the last argument. You can also use $_ to get the last argument of the previous command. |

## Lines in history

||Description|
| -- | -- |
| !-1 | previous command |
| !-2, !-3, ... | are earlier commands and arguments can be extracted from earlier commands too |
| !-2^, !-2:2, !-2$, !-2* | you can extract particular arguments as well. |

## Word designators

Generally a colon (:) separates the event designator and the word designator, but it can be omitted, if the word designator begins with $, %, ^, *, or - . As well for for command line arguments from 1 to 9 ($1-$9)

||Description|
| -- | -- |
| 0 | the zero’th word (command name) |
| n | word n |
| ˆ | the first argument, i.e., word one (usually first argument of the command) |
| $ | the last argument |
| % | the word matched by the most recent |
| !? str ?  |search of str |
| x-y | words x through y . −y is short for 0−y |
| * | words 1 through the last (like 1−$ ) (all arguments) |
| n* | words n through the last (like n−$ ) |

## Modifiers:

||Description|
| -- | -- |
| h | remove the part of a filename after last "/", leaving the path |
| t | remove all but the part of the filename after the last slash. for example for "/etc/resolv.conf" it will be resolv.conf |
| r | remove the last suffix of a filename (extension of the file). For example /etc/resolv |
| e | remove all but the last suffix of a filename (extension) |
| g | make changes globally, use with s modifier, below |
| p | print the command but do not execute it |
| q | quote the generated text |
| s/old/new/ | substitute new for old in the text. Any delimiter may be used. An & in the argument means the value of old. With empty old , use last old , or the most recent !? str ? search if there was no previous old |
| x | quote the generated text, but break into words at blanks and newline |
| & | repeat the last substitution |

**Example:**

```bash
echo this is a phrase
!:s/this/that<tab>
```

- [Bash history reuse and bang commands](http://www.softpanorama.org/Scripting/Shellorama/Bash_as_command_interpreter/bash_command_history_reuse.shtml#Using_selected_capabilities_of_bang_command)
- [Bash Bang (!) Commands - Linux - SS64.com](https://ss64.com/bash/bang.html)
- [Unix history and bang commands - Jack Hsu](https://jaysoo.ca/2009/09/16/unix-history-and-bang-commands/)

# Send command to all panes

    prefix :set-window-option synchronize-panes on|off

On a default configuration:

    ctrl-b :set-window-option synchronize-panes on

Credits to: [tmux send command to all panes](http://openbsd-archive.7691.n7.nabble.com/tmux-send-command-to-all-panes-td95883.html)

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

# Wifi after hibernation

Change `/etc/NetworkManager/conf.d/default-wifi-powersave-on.conf` to be:

```
[connection]
wifi.powersave = 2
```

Don't know if it really helped anything, but at least now I know that I have wifi after hibernation.

# Download a video from youtube

To download a video from youtube:

    youtube-dl --list-formats $youtube_link

After seeing the list of options and formats

    youtube-dl -f $format $youtube_link

e.g. for download only the audio:

    youtube-dl -f 140 $youtube_link

# Convert the downloaded audio

When you download the audio it have an `m4a` extension, to convert to a `mp3` audio file:

    ffmpeg -i $m4a_file -acodec libmp3lame -ab 128k ${m4a_file%.*}.mp3

To to a batch conversion, and have safe names:

    for file in *m4a; do rename 's/ /_/g' $file; done && for file in *m4a; do ffmpeg -i $file -acodec libmp3lame -ab 128k ${file%.*}.mp3; done


# Create a ramdisk

    mkdir -p /mnt/ramdisk && mount -t tmpfs tmpfs /mnt/ramdisk -o size=8192M

# Get your current IPs

    curl ifconfig.me

    curl ipinfo.io/ip

# Resetting the gpg-agent

If, by any reason, gpg stops working you can kill it:

    gconf --kill gpg-agent

GPG will restart next time it is needed.

Tip from https://superuser.com/a/1150399

# VirtualBox shared folder

If you are having permission denied issues on VirtualBox shared folders:

```
sudo usermod -aG vboxsf $(whoami)
```
