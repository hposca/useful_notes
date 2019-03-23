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
