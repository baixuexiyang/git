# NAME
git-branch - List branches

#DESCRIPTION 
If --list is given, or if there are no non-option arguments, existing branches are listed; the current branch will be highlighted with an asterisk. Option -r causes the remote-tracking branches to be listed, and option -a shows both local and remote branches. If a &lt;pattern&gt; is given, it is used as a shell wildcard to restrict the output to matching branches. If multiple patterns are given, a branch is shown if it matches any of the patterns. Note that when providing a &lt;pattern&gt;, you must use --list; otherwise the command is interpreted as branch creation.

With --contains, shows only the branches that contain the named commit (in other words, the branches whose tip commits are descendants of the named commit). With --merged, only branches merged into the named commit (i.e. the branches whose tip commits are reachable from the named commit) will be listed. With --no-merged only branches not merged into the named commit will be listed. If the &lt;commit&gt; argument is missing it defaults to HEAD (i.e. the tip of the current branch).

# OPTIONS  
-r  
--remotes  
List or delete (if used with -d) the remote-tracking branches.

-a  
--all  
List both remote-tracking branches and local branches.

--list   
Activate the list mode. git branch &lt;pattern&gt; would try to create a branch, use git branch --list &lt;pattern&gt; to list matching branches.

#Examples

show the local branchs

``` bash
git branch
```
or 
``` bash
git branch --list
```

show the remote branches  

``` bash
git branch -r
```

show the all branches

``` bash
git branch -a
```
