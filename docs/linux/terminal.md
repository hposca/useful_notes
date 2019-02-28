# Displaying a function definition

If you have a bash defined funcion, e.g. foobar, and want to display its definition you can:

`type foobar` To discover its location
`declare -f foobar` To display its definition

Found this info at [Can bash show a function's definition? - Stack Overflow](https://stackoverflow.com/questions/6916856/can-bash-show-a-functions-definition).

# Bang commands

||Description|
| -- | -- |
| !! | Will execute the last command. Same as !-1 or "up-arrow + return". The most common use is sudo !! when issued command failed due to insufficient privileges. |
| !n | Will execute the line n of the history record.   In most cases using mouse is a better deal |
!-n | Will execute the command n lines back.  You usually remember the last three-four command you issues so !-2 and !-3 speed things up. |
| !gzip | will re-execute the most recent gzip command (history is searched  in reverse order). String specified is considered to be the prefix of the  command. |
| !?etc.gz | same as above but the unique string doesn't have to be at the start of the command. That means that you can identify command by a unique string appearing anywhere in the command  (might be faster then Ctrl-R  mechanism) |
| !!:1 | designates the first argument of the last command.  This can be shortened to !1. |
| !!:$ | designates the last argument of the preceding command. This may be shortened to !$. |


I think you need you need to know  five major bang commands (!!, !!:h, !!:f, !$, !1)

## Useful idioms

||Description|
| -- | -- |
| sudo !! | reexecute the last command with prefix sudo. |
| cd !$ | cd to the directory which is the last argument of the previous command. |


## Retrieving previous command arguments

||Description|
| -- | -- |
| !:0 | is the previous command name |
| !^, !:2, !:3, …, !$ | are the arguments of the previous command |
| !* | is all the arguments |
| !$ | is the last argument. You can also use $_ to get the last argument of the previous command. |

## Lines in history

||Description|
| -- | -- |
| !-1 | previous command |
| !-2, !-3, ... | are earlier commands and arguments can be extracted from earlier commands too |
| !-2^, !-2:2, !-2$, !-2* | you can extract particular arguments as well. |

## Word designators

Generally a colon (:) separates the event designator and the word designator, but it can be omitted, if the word designator begins with $, %, ^, *, or - . As well for for command line arguments from 1 to 9 ($1-$9)

||Description|
| -- | -- |
| 0 | the zero’th word (command name) |
| n | word n |
| ˆ | the first argument, i.e., word one (usually first argument of the command) |
| $ | the last argument |
| % | the word matched by the most recent |
| !? str ?  |search of str |
| x-y | words x through y . −y is short for 0−y |
| * | words 1 through the last (like 1−$ ) (all arguments) |
| n* | words n through the last (like n−$ ) |

## Modifiers:

||Description|
| -- | -- |
| h | remove the part of a filename after last "/", leaving the path |
| t | remove all but the part of the filename after the last slash. for example for "/etc/resolv.conf" it will be resolv.conf |
| r | remove the last suffix of a filename (extension of the file). For example /etc/resolv |
| e | remove all but the last suffix of a filename (extension) |
| g | make changes globally, use with s modifier, below |
| p | print the command but do not execute it |
| q | quote the generated text |
| s/old/new/ | substitute new for old in the text. Any delimiter may be used. An & in the argument means the value of old. With empty old , use last old , or the most recent !? str ? search if there was no previous old |
| x | quote the generated text, but break into words at blanks and newline |
| & | repeat the last substitution |

**Example:**

```bash
echo this is a phrase
!:s/this/that<tab>
```

- [Bash history reuse and bang commands](http://www.softpanorama.org/Scripting/Shellorama/Bash_as_command_interpreter/bash_command_history_reuse.shtml#Using_selected_capabilities_of_bang_command)
- [Bash Bang (!) Commands - Linux - SS64.com](https://ss64.com/bash/bang.html)
- [Unix history and bang commands - Jack Hsu](https://jaysoo.ca/2009/09/16/unix-history-and-bang-commands/)
