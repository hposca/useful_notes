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
