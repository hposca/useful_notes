[xorg - List all valid kbd layouts, variants and toggle options (to use with setxkbmap)](https://unix.stackexchange.com/a/298947):

        Take a look at localectl, especially following options:

        localectl list-x11-keymap-layouts - gives you layouts (~100 on modern systems)
        localectl list-x11-keymap-variants de - gives you variants for this layout (or all variants if no layout specified, ~300 on modern systems)
        localectl list-x11-keymap-options | grep grp: - gives you all layout switching options
