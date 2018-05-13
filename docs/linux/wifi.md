# Wifi after hibernation

Change `/etc/NetworkManager/conf.d/default-wifi-powersave-on.conf` to be:

```
[connection]
wifi.powersave = 2
```

Don't know if it really helped anything, but at least now I know that I have wifi after hibernation.
