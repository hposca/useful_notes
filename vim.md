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

# Abrindo um arquivo em hexadecimal

Depois de abrir o arquivo, para se ver o hex dump dele:

~~~ Bash
:%!xxd
~~~

Depois de editar o hexadecimal, para converter o arquivo devolta ao formato binário:

~~~ Bash
:%!xxd -r
~~~
