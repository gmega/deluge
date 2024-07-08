# Git Source Code Repository

Deluge's source code is always available through our [Git repository](http://git.deluge-torrent.org/deluge).

To get a copy of the code, you will need to clone it from the repository then see the [Install from source](/installing/source.md) guide.

For more in-depth git usage see [GitTips](/development/git/gittips.md).

This guide uses commands which are meant to be typed at a command prompt, but most will be available from a GUI Git client.

[Install Git](http://git-scm.com/downloads)



## Deluge Branches
There are multiple branches or tags that you can choose from which contain different stages of the Deluge code.

Note that we are currently transitioning to a different [git workflow](http://nvie.com/posts/a-successful-git-branching-model/) with the development of *1.4* so picking the correct branch is crucial.

The current branches (at time of writing) are:

 **`1.3-stable`**::
    This is the release branch for the 1.3-series and, until 1.4 release, is the recommended stable code. This branch is only for critical bug fixes and no new feature will appear in this branch.
 **`master`**::
    ***This branch is in limbo until 1.4 release*** This branch contains development code that is now continued in `develop`. When the `develop` code is considered stable, 1.4 will be released and the code will be merged back into `master` code. This mean going forward from 1.4 release this branch will be considered to be the most stable code, replacing the use of *-stable branches.
 **`develop`**::
    All development work will be put into this branch and should be considered unstable with risk of data loss and potential incompatibility with other Deluge versions.
 **`extjs4-port`**::
   A branch for porting the WebUI code from `Ext JS 3.4` to `Ext JS 4`, still requiring a lot of development work.

## Initial Clone

The first step is to clone our git repos:

```
git clone git://deluge-torrent.org/deluge.git
```

This will create a *deluge* directory with a copy of the repo so if you change into this directory you can start using git commands.


## Select Branch

List the branches, including remote branches:

```
git branch -a
```

## Switch Branch

If you want to use a different branch than the default selected (`master`) then you will need to create a local copy of the remote branch. In this example we create branch '1.3-stable' as a local copy of the remote branch 'origin/1.3-stable' and switch to it:

```
git checkout -b 1.3-stable origin/1.3-stable
```

To switch between your local branches use the following commands:

 **Development:**

```
git checkout develop
```

 **Stable:**

```
git checkout 1.3-stable
```

## Show current Branch
Either of these commands will show the current branch you are using:

```
git branch
git status
```


## Updating

You only need to do a clone once, after that you can simply update the branch by *pulling* changes from the repo.

Assuming you are in the folder you cloned to:

```
git pull
```

Now your tree is synchronized to the latest revision in our repository!