# Dicas gerais para vim

# Vimdiff

    Two commands can be used to jump to diffs:

                                *[c*
    [c		Jump backwards to the previous start of a change.
            When a count is used, do it that many times.
                                *]c*
    ]c		Jump forwards to the next start of a change.
            When a count is used, do it that many times.

    There are two commands to copy text from one buffer to another.  The result is
    that the buffers will be equal within the specified range.

                                *do*
    do		Same as ":diffget" without argument or range.  The "o" stands
            for "obtain" ("dg" can't be used, it could be the start of
            "dgg"!). Note: this doesn't work in Visual mode.

                                *dp*
    dp		Same as ":diffput" without argument or range.
            Note: this doesn't work in Visual mode.

## Scroll simultâneo

~~~ Bash
set scrollbind   # Para ativar
set noscrollbind # Para desativar
~~~

# Abrindo um arquivo em hexadecimal

Depois de abrir o arquivo, para se ver o hex dump dele:

~~~ Bash
:%!xxd
~~~

Depois de editar o hexadecimal, para converter o arquivo devolta ao formato binário:

~~~ Bash
:%!xxd -r
~~~

# Repetindo macros

Execute the macro stored in register a on lines 5 through 10.

~~~ Bash
:5,10norm! @a
~~~

Execute the macro stored in register a on all lines.

~~~ Bash
:%norm! @a
~~~

# Editando macros

Dicas [desse site](https://robots.thoughtbot.com/how-to-edit-an-existing-vim-macro)

Yanking into a register

    "qp paste the contents of the register to the current cursor position
    I enter insert mode at the begging of the pasted line
    ^ add the missing motion to return to the front of the line
    <Escape> return to visual mode
    "qyy yank this new modified macro back into the q register
    dd delete the pasted register from the file your editing

Editing the register visually

    :let @q=' open the q register
    <Cntl-r><Cntl-r>q paste the contents of the q register into the buffer
    ^ add the missing motion to return to the front of the line
    ' add a closing quote
    <Enter> finish editing the macro

# Correção ortográfica

~~~ Bash
set spell spelllang=en_ca      # Ativando globalmente
setlocal spell spelllang=en_ca # Ativando localmente
set nospell                    # Desativando

]s                             # move the cursor to the next misspelled word
[s                             # move the cursor back to previous misspelled words.

z=                             # Vim will suggest a list of alternatives that it thinks may be correct

zg                             # Vim will add the word to its dictionary
zw                             # Mark word as incorrect
~~~

# Substituindo ^M por quebras de linha

With help from [here](http://stackoverflow.com/questions/811193/how-to-convert-the-m-linebreak-to-normal-linebreak-in-a-file-opened-in-vim):

    :%s/<Ctrl-V><Ctrl-M>/\r/g

# Copy the filename/full path of a file

With this, it will copy the filename or full file path into the `"` register, which is the vim's default clipboard.

Filename:

```
:let @" = expand("%")
```

Full path:

```
:let @" = expand("%:p")
```
Thanks to [clipboard - Yank file name / path of current buffer in Vim - Stack Overflow](https://stackoverflow.com/questions/916875/yank-file-name-path-of-current-buffer-in-vim).

And if you want to copy only the full-path of the directory:

```
:let @" = expand("%:p:h")
```

Thanks to [How can I expand the full path of the current file to pass to a command in Vim? - Stack Overflow](https://stackoverflow.com/questions/2233905/how-can-i-expand-the-full-path-of-the-current-file-to-pass-to-a-command-in-vim).

More info can be found at `:help filename-modifiers`.

# Sorting IPs

From this [StackOverflow question](https://stackoverflow.com/questions/9067559/sorting-ip-addresses-in-vim#answer-9075090):

```
:%s/\<\d\d\?\>/0&/g|%&&|sor r/\(\d\{3}\)\%(\.\d\{3}\)\{3}/|%s/\<00\?\ze\d//g
```

# Execute current line as Vim EX command

From [How can I execute the current line as Vim EX commands? - Stack Overflow](https://stackoverflow.com/questions/14385998/how-can-i-execute-the-current-line-as-vim-ex-commands/14386090)

```
yy:@"
```

Or

```
Y:@"
```

Have to <enter> to begin the execution

This was super useful when I had to execute many different substitutions on a list of files.
