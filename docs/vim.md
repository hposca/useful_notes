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
