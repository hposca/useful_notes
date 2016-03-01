# Find and Replace string in all files

From [Find and Replace string in all files recursive using grep and sed](http://stackoverflow.com/questions/15920276/find-and-replace-string-in-all-files-recursive-using-grep-and-sed):

    grep -rl $oldstring /path/to/folder | xargs sed -i s@$oldstring@$newstring@g
