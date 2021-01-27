
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
