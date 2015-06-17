Undoing mistakes

# Find and restore a deleted file

With the help of [StackOverflow](http://stackoverflow.com/questions/953481/find-and-restore-a-deleted-file-in-a-git-repo):

Find the last commit that affected the given path. As the file isn't in the HEAD commit, this commit must have deleted it.

~~~ bash
git rev-list -n 1 HEAD -- <file_path>
~~~

Then checkout the version at the commit before, using the caret (^) symbol:

~~~ bash
git checkout <deleting_commit>^ -- <file_path>
~~~

Or in one command, if `$file` is the file in question.

~~~ bash
git checkout $(git rev-list -n 1 HEAD -- "$file")^ -- "$file"
~~~
