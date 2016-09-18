#NAME
git-branch - delete branches   

#DESCRIPTION
The branch must be fully merged in its upstream branch, or in HEAD if no upstream was set with --track or --set-upstream.   

# OPTIONS
-d   
--delete   
Delete a branch. The branch must be fully merged in its upstream branch, or in HEAD if no upstream was set with --track or --set-upstream.

-D   
Shortcut for --delete --force.  

# Examples  
If you want to delete the local branch, you can do like this:
``` bash
$ git branch -d <branch>
```
> note this command will not delete the origin branch

If you want delete the remote branch, you can do like this:   

``` bash
$ git push origin --delete <branch>
```

Also, delete the remote-tracking branches "todo", "html" and "man". The next fetch or pull will create them again unless you configure them not to.   
``` bash
$ git branch -d -r origin/todo origin/html origin/man
```

Delete the "test" branch even if the "master" branch (or whichever branch is currently checked out) does not have all commits from the test branch.   
``` bash
$ git branch -D <branch>   
```
