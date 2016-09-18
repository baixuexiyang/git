# NAME
git-merge - Join two or more development histories together

# SYNOPSIS
``` bash
git merge [-n] [--stat] [--no-commit] [--squash] [--[no-]edit]
	[-s <strategy>] [-X <strategy-option>] [-S[<keyid>]]
	[--[no-]rerere-autoupdate] [-m <msg>] [<commit>…​]
git merge <msg> HEAD <commit>…​
git merge --abort
```
# DESCRIPTION
Incorporates changes from the named commits (since the time their histories diverged from the current branch) into the current branch. This command is used by git pull to incorporate changes from another repository and can be used by hand to merge changes from one branch into another.

Assume the following history exists and the current branch is "master":
```
	  A---B---C topic
	 /
    D---E---F---G master
```
Then "git merge topic" will replay the changes made on the topic branch since it diverged from master (i.e., E) until its current commit (C) on top of master, and record the result in a new commit along with the names of the two parent commits and a log message from the user describing the changes.
```
	  A---B---C topic
	 /         \
    D---E---F---G---H master
```
The second syntax (&lt;msg&gt; HEAD &lt;commit&gt;…​) is supported for historical reasons. Do not use it from the command line or in new scripts. It is the same as git merge -m &lt;msg&gt; &lt;commit&gt;....

The third syntax ("git merge --abort") can only be run after the merge has resulted in conflicts. git merge --abort will abort the merge process and try to reconstruct the pre-merge state. However, if there were uncommitted changes when the merge started (and especially if those changes were further modified after the merge was started), git merge --abort will in some cases be unable to reconstruct the original (pre-merge) changes. Therefore:

Warning: Running git merge with non-trivial uncommitted changes is discouraged: while possible, it may leave you in a state that is hard to back out of in the case of a conflict.

# OPTIONS
--commit   
--no-commit   
Perform the merge and commit the result. This option can be used to override --no-commit.

With --no-commit perform the merge but pretend the merge failed and do not autocommit, to give the user a chance to inspect and further tweak the merge result before committing.

--edit  
-e   
--no-edit   
Invoke an editor before committing successful mechanical merge to further edit the auto-generated merge message, so that the user can explain and justify the merge. The --no-edit option can be used to accept the auto-generated message (this is generally discouraged). The --edit (or -e) option is still useful if you are giving a draft message with the -m option from the command line and want to edit it in the editor.

Older scripts may depend on the historical behaviour of not allowing the user to edit the merge log message. They will see an editor opened when they run git merge. To make it easier to adjust such scripts to the updated behaviour, the environment variable GIT_MERGE_AUTOEDIT can be set to no at the beginning of them.

--ff   
When the merge resolves as a fast-forward, only update the branch pointer, without creating a merge commit. This is the default behavior.

--no-ff  
Create a merge commit even when the merge resolves as a fast-forward. This is the default behaviour when merging an annotated (and possibly signed) tag.

--ff-only  
Refuse to merge and exit with a non-zero status unless the current HEAD is already up-to-date or the merge can be resolved as a fast-forward.

--log[=&lt;n&gt;]  
--no-log    
In addition to branch names, populate the log message with one-line descriptions from at most <n> actual commits that are being merged. See also git-fmt-merge-msg(1).

With --no-log do not list one-line descriptions from the actual commits being merged.

--stat  
-n  
--no-stat  
Show a diffstat at the end of the merge. The diffstat is also controlled by the configuration option merge.stat.

With -n or --no-stat do not show a diffstat at the end of the merge.

--squash  
--no-squash  
Produce the working tree and index state as if a real merge happened (except for the merge information), but do not actually make a commit, move the HEAD, or record $GIT_DIR/MERGE_HEAD (to cause the next git commit command to create a merge commit). This allows you to create a single commit on top of the current branch whose effect is the same as merging another branch (or more in case of an octopus).

With --no-squash perform the merge and commit the result. This option can be used to override --squash.

-s &lt;strategy&gt;  
--strategy=&lt;strategy&gt;  
Use the given merge strategy; can be supplied more than once to specify them in the order they should be tried. If there is no -s option, a built-in list of strategies is used instead (git merge-recursive when merging a single head, git merge-octopus otherwise).

-X &lt;option&gt;  
--strategy-option=&lt;option&gt;  
Pass merge strategy specific option through to the merge strategy.

--verify-signatures   
--no-verify-signatures  
Verify that the commits being merged have good and trusted GPG signatures and abort the merge in case they do not.

--summary  
--no-summary  
Synonyms to --stat and --no-stat; these are deprecated and will be removed in the future.

-q  
--quiet  
Operate quietly. Implies --no-progress.

-v   
--verbose  
Be verbose.

--progress   
--no-progress   
Turn progress on/off explicitly. If neither is specified, progress is shown if standard error is connected to a terminal. Note that not all merge strategies may support progress reporting.

-S[&lt;keyid&gt;]   
--gpg-sign[=&lt;keyid&gt;]   
GPG-sign the resulting merge commit. The keyid argument is optional and defaults to the committer identity; if specified, it must be stuck to the option without a space.

-m &lt;msg&gt;   
Set the commit message to be used for the merge commit (in case one is created).

If --log is specified, a shortlog of the commits being merged will be appended to the specified message.

The git fmt-merge-msg command can be used to give a good default for automated git merge invocations. The automated message can include the branch description.

--[no-]rerere-autoupdate  
Allow the rerere mechanism to update the index with the result of auto-conflict resolution if possible.

--abort  
Abort the current conflict resolution process, and try to reconstruct the pre-merge state.

If there were uncommitted worktree changes present when the merge started, git merge --abort will in some cases be unable to reconstruct these changes. It is therefore recommended to always commit or stash your changes before running git merge.

git merge --abort is equivalent to git reset --merge when MERGE_HEAD is present.

&lt;commit&gt;…​  
Commits, usually other branch heads, to merge into our branch. Specifying more than one commit will create a merge with more than two parents (affectionately called an Octopus merge).

If no commit is given from the command line, merge the remote-tracking branches that the current branch is configured to use as its upstream. See also the configuration section of this manual page.

When FETCH_HEAD (and no other commit) is specified, the branches recorded in the .git/FETCH_HEAD file by the previous invocation of git fetch for merging are merged to the current branch.

#PRE-MERGE CHECKS  
Before applying outside changes, you should get your own work in good shape and committed locally, so it will not be clobbered if there are conflicts. See also git-stash(1). git pull and git merge will stop without doing anything when local uncommitted changes overlap with files that git pull/git merge may need to update.

To avoid recording unrelated changes in the merge commit, git pull and git merge will also abort if there are any changes registered in the index relative to the HEAD commit. (One exception is when the changed index entries are in the state that would result from the merge already.)

If all named commits are already ancestors of HEAD, git merge will exit early with the message "Already up-to-date."

# FAST-FORWARD MERGE
Often the current branch head is an ancestor of the named commit. This is the most common case especially when invoked from git pull: you are tracking an upstream repository, you have committed no local changes, and now you want to update to a newer upstream revision. In this case, a new commit is not needed to store the combined history; instead, the HEAD (along with the index) is updated to point at the named commit, without creating an extra merge commit.

This behavior can be suppressed with the --no-ff option.

# TRUE MERGE
Except in a fast-forward merge (see above), the branches to be merged must be tied together by a merge commit that has both of them as its parents.

A merged version reconciling the changes from all branches to be merged is committed, and your HEAD, index, and working tree are updated to it. It is possible to have modifications in the working tree as long as they do not overlap; the update will preserve them.

When it is not obvious how to reconcile the changes, the following happens:

The HEAD pointer stays the same.

The MERGE_HEAD ref is set to point to the other branch head.

Paths that merged cleanly are updated both in the index file and in your working tree.

For conflicting paths, the index file records up to three versions: stage 1 stores the version from the common ancestor, stage 2 from HEAD, and stage 3 from MERGE_HEAD (you can inspect the stages with git ls-files -u). The working tree files contain the result of the "merge" program; i.e. 3-way merge results with familiar conflict markers <<< === >>>.

No other changes are made. In particular, the local modifications you had before you started merge will stay the same and the index entries for them stay as they were, i.e. matching HEAD.

If you tried a merge which resulted in complex conflicts and want to start over, you can recover with git merge --abort.

#EXAMPLES  
Merge branches fixes and enhancements on top of the current branch, making an octopus merge:
``` bash
$ git merge fixes enhancements
```
Merge branch obsolete into the current branch, using ours merge strategy:
``` bash
$ git merge -s ours obsolete
```
Merge branch maint into the current branch, but do not make a new commit automatically:
``` bash
$ git merge --no-commit maint
```
This can be used when you want to include further changes to the merge, or want to write your own merge commit message.

You should refrain from abusing this option to sneak substantial changes into a merge commit. Small fixups like bumping release/version name would be acceptable.
