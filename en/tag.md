# NAME
git-tag - Create, list, delete or verify a tag object signed with GPG

# SYNOPSIS
``` bash
git tag [-a | -s | -u &lt;keyid&gt;] [-f] [-m <msg> | -F <file>]
	<tagname> [<commit> | <object>]
git tag -d <tagname>…​
git tag [-n[<num>]] -l [--contains <commit>] [--points-at <object>]
	[--column[=<options>] | --no-column] [--create-reflog] [<pattern>…​]
git tag -v <tagname>…​
```

# DESCRIPTION
Add a tag reference in refs/tags/, unless -d/-l/-v is given to delete, list or verify tags.

Unless -f is given, the named tag must not yet exist.

If one of -a, -s, or -u &lt;keyid&gt; is passed, the command creates a tag object, and requires a tag message. Unless -m &lt;msg&gt; or -F &lt;file&gt; is given, an editor is started for the user to type in the tag message.

If -m &lt;msg&gt; or -F &lt;file&gt; is given and -a, -s, and -u &lt;keyid&gt; are absent, -a is implied.

Otherwise just a tag reference for the SHA-1 object name of the commit object is created (i.e. a lightweight tag).

A GnuPG signed tag object will be created when -s or -u &lt;keyid&gt; is used. When -u &lt;keyid&gt; is not used, the committer identity for the current user is used to find the GnuPG key for signing. The configuration variable gpg.program is used to specify custom GnuPG binary.

Tag objects (created with -a, -s, or -u) are called "annotated" tags; they contain a creation date, the tagger name and e-mail, a tagging message, and an optional GnuPG signature. Whereas a "lightweight" tag is simply a name for an object (usually a commit object).

Annotated tags are meant for release while lightweight tags are meant for private or temporary object labels. For this reason, some git commands for naming objects (like git describe) will ignore lightweight tags by default.

# OPTIONS
-a  
--annotate  
Make an unsigned, annotated tag object

-s  
--sign   
Make a GPG-signed tag, using the default e-mail address’s key.

-u &lt;keyid&gt;  
--local-user=&lt;keyid&gt;   
Make a GPG-signed tag, using the given key.

-f  
--force  
Replace an existing tag with the given name (instead of failing)

-d  
--delete  
Delete existing tags with the given names.

-v  
--verify  
Verify the gpg signature of the given tag names.

-n&lt;num&gt;  
&lt;num&gt; specifies how many lines from the annotation, if any, are printed when using -l. The default is not to print any annotation lines. If no number is given to -n, only the first line is printed. If the tag is not annotated, the commit message is displayed instead.

-l &lt;pattern&gt;  
--list &lt;pattern&gt;  
List tags with names that match the given pattern (or all if no pattern is given). Running "git tag" without arguments also lists all tags. The pattern is a shell wildcard (i.e., matched using fnmatch(3)). Multiple patterns may be given; if any of them matches, the tag is shown.

--sort=&lt;type&gt;  
Sort in a specific order. Supported type is "refname" (lexicographic order), "version:refname" or "v:refname" (tag names are treated as versions). The "version:refname" sort order can also be affected by the "versionsort.prereleaseSuffix" configuration variable. Prepend "-" to reverse sort order. When this option is not given, the sort order defaults to the value configured for the tag.sort variable if it exists, or lexicographic order otherwise. See git-config(1).

--column[=&lt;options&gt;]  
--no-column  
Display tag listing in columns. See configuration variable column.tag for option syntax.--column and --no-column without options are equivalent to always and never respectively.

This option is only applicable when listing tags without annotation lines.

--contains [&lt;commit&gt;]  
Only list tags which contain the specified commit (HEAD if not specified).

--points-at &lt;object&gt;  
Only list tags of the given object.

-m &lt;msg&gt;  
--message=&lt;msg&gt;  
Use the given tag message (instead of prompting). If multiple -m options are given, their values are concatenated as separate paragraphs. Implies -a if none of -a, -s, or -u &lt;keyid&gt; is given.

-F &lt;file&gt;  
--file=&lt;file&gt;  
Take the tag message from the given file. Use - to read the message from the standard input. Implies -a if none of -a, -s, or -u &lt;keyid&gt; is given.

--cleanup=&lt;mode&gt;  
This option sets how the tag message is cleaned up. The &lt;mode&gt; can be one of verbatim, whitespace and strip. The strip mode is default. The verbatim mode does not change message at all, whitespace removes just leading/trailing whitespace lines and strip removes both whitespace and commentary.

--create-reflog  
Create a reflog for the tag.

&lt;tagname&gt;  
The name of the tag to create, delete, or describe. The new tag name must pass all checks defined by git-check-ref-format(1). Some of these checks may restrict the characters allowed in a tag name.

&lt;commit&gt;  
&lt;object&gt;  
The object that the new tag will refer to, usually a commit. Defaults to HEAD.

# CONFIGURATION  
By default, git tag in sign-with-default mode (-s) will use your committer identity (of the form Your Name &lt;your@email.address&gt;) to find a key. If you want to use a different default key, you can specify it in the repository configuration as follows:
``` bash
[user]
    signingKey = &lt;gpg-keyid&gt;
```
