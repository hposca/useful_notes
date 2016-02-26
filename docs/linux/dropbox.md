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
