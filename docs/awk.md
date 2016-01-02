# Print n-th column to last column

From [this StackOverflow thread](http://stackoverflow.com/questions/1602035/print-third-column-to-last-column), this is how you can print from the n-th column to the last column of a line, using awk.

e.g. form the third column:

    awk '{ print substr($0, index($0,$3)) }'

## Use cases

This is useful with you want to analyse your own command's history, like:

    history | awk '{print substr($0, index($0,$2))}'
