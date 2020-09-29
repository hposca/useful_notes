# Hibernation Problems

After installing Linux Mint 20 Hibernation didn't work.

To make it work I followed [this post](https://forums.linuxmint.com/viewtopic.php?p=1495203&sid=9b5eeffffe5ffd2073e04adbb276d15e#p1495203) on how to update grub and merging [this post's](https://forums.linuxmint.com/viewtopic.php?p=1854165#p1854165) `Action`s of the `com.ubuntu.enable-hibernate.pkla` file.

And creating the service to disable devices to wake up from hibernation as this [SO guide](https://unix.stackexchange.com/questions/602406/guide-on-how-to-enable-hibernation-on-linux-mint-20-cinnamon-ubuntu-20-and-pre), in my case I wanted to disable the keyboard from waking up the computer.
