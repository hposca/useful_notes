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
