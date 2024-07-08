# Git Tips


This page is a collection of useful git tips.

## Merge a feature branch

In this example we assume that your feature branch, `myfeature`, is based off `develop` and that you are currently in your feature branch.

1. Squash/reword/edit any commits if need be

```sh
git rebase -i develop
```
1. Rebase your feature branch on top of current develop

```sh
git rebase develop
```
1. Change to develop

```sh
git checkout develop
```
1. `Fast-forward` merge your feature branch into develop

```sh
git merge myfeature
```
1. Delete your local feature branch (see above tip for remote deletion)

```sh
git branch -d myfeature
```

## Ignore changes in a tracked file

To ignore:

```sh
git update-index --assume-unchanged <tracked-file>
```

To stop ignoring:

```sh
git update-index --no-assume-unchanged <ignored-tracked-file>
```

## Undo the last commit
*Changes are put back into staged*

```sh
git reset --soft HEAD~1
```


## Delete the last commit
*All changes are lost! *

```sh
git reset --hard HEAD~1
```

## Stash untracked files

At times you will be working on a branch with untracked files (new files) and you'll need to checkout master to apply a bug fix.

First, add the untracked files to the index:

```sh
git add <untracked-files>
```

Now do a regular stash:

```sh
git stash
```

At this point you can checkout whatever branch you want and do what you need to do.  When you're done, checkout your original working branch again and pop the changes from your stash:

```sh
git stash pop
```

You can remove the files you indexed from the index if you want, as to not pollute your next commit:

```sh
git rm --cached <files>
```

## Merge a remote branch

First, add the repo:

```sh
git remote add <reponame> <location>
```

Now fetch in the changes:

```sh
git fetch <reponame>
```

You'll want to do a diff against your branch to see what will be merged:

```sh
git diff master <reponame>/<branch>
```

If you're happy with the changes, go ahead and merge it:

```sh
git merge <reponame>/<branch>
```


## Delete a remote branch

```sh
git push <repo> :<branch>
```

or

```sh
git push --delete <repo> <branch>
```

## Apply a commit to multiple branches with cherry-pick

This technique uses cherry-pick to apply commits to branches that differ too much to use `merge`.

1. Checkout `master` branch

```sh
git checkout master
```
1. Make commit

```sh
git commit -m "Fixed the bug that caused issue xyz"
```
1. Checkout stable branch

```sh
git checkout stable
```
1. Apply the last commit from master to this branch

```sh
git cherry-pick master
```

## Commit Messages

```comment
This should probably have it's own page but putting here for now.
```
Based on [git commit guidelines](http://git-scm.com/book/en/Distributed-Git-Contributing-to-a-Project#Commit-Guidelines) and [How to Write a Git Commit Message](http://chris.beams.io/posts/git-commit/).

The short answer on the golden rule for git commit subject lines is to you ask yourself:
  **If applied, this commit will...** *<your commit subject line here>*


e.g. If applied, this commit will `Enable editing of torrent names in Add Dialog`

A general guideline on writing a good commit message is to provide information
that is not already provided from the commit diff, which in essence is "*the why, not the how*".

* Explain why the change is necessary
  * *After reading the commit message it should should be apparent why the changes were made.*

* Explain how the issue is addressed in broad terms
  * *It is not necessary to explain how the changes are made in detail as that can be seen from the commit diff.*

Example commit message:

```
Short summary of changes (preferably 50 characters or less)

More detailed explanatory text, if necessary.  Wrap it to about 72
characters or so. The first line can be considered as the subject of an
email and the rest of the text as the body. The blank line separating
the summary from the body is critical, unless you omit the body entirely.

Further paragraphs come after blank lines.

  - Bullet points are okay, too

  - Typically a hyphen or asterisk is used for the bullet, preceded by a
    single space, with blank lines in between, but conventions vary here
```

### Tags / Labels
To differentiate commit changes at a glance, the subject of commits for specific components should be prefixed with relevant tags/labels, such as:
 *Core*, *WebUI*, *GTKUI*, *Tests*, *Packaging*, *Console*, *Blocklist*, *Lint* etc. (see git logs for more examples)

`[GTKUI] Enable editing of torrent names in Add Dialog`

If the commit fix/closes a ticket, include the number:

`[[#1001](https://dev.deluge-torrent.org/ticket/1001)] Add support for magnet uris`

A limited number of tags may be combined, such as

`[[#1002](https://dev.deluge-torrent.org/ticket/1002)] [GTKUI] Fix the files tab context menu not being displayed`

`[[#2250](https://dev.deluge-torrent.org/ticket/2250)] [Core] [GTKUI] Add method remove_torrents to core`

