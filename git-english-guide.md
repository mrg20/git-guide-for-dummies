# Git guidelines to follow

In this document, you will be able to find git commands, accompanied by their use and explanation. These commands are grouped into three levels, as not all of them are as simple or easy to understand, nor as important. With the first two levels, you will be able to work with the repository.

## Index
### Part 1 (Local)
* [Git structure](#git-structure)
* [Git add](#git-add)
* [Git commit](#git-commit)
* [Git status](#git-status)
* [Git branch](#git-branch)
* [Git checkout](#git-checkout)
* [Git merge](#git-merge)

### Part 2 (Origin part 1)
* [Git clone](#git-clone)
* [Origin](#origin)
* [Git pull origin branch](#git-pull-origin-branch)
* [Git push origin branch](#git-push-origin-branch)

### Part 3 (Origin part 2)
* [Git fetch](#git-fetch)
* [Git pull](#git-pull)
* [Git revert](#git-revert)

## Part 1
At this level, no detail is given about any of the explained concepts. Only a brief overview is provided to understand what is being done.

### Git structure
In essence, Git is a tree. This tree is formed by two general concepts that someone must know like the back of one's hand: Branches and commits.

**Commit:**
A commit is a version of our program, that we have saved.

*Example*
```
|____ i18n <= A commit
| |___ file 'pot.js' modified
| |___ file 'index.html' deleted
|____ a32c <= Another commit
        |___ file 'router.ts' created
```

**Branch:**
A branch is a chain of commits. In the previous case, it was the master branch. 

*Example*
```
master
|____ i18n <= A commit
| |___ file 'pot.js' modified
| |___ file 'index.html' deleted
|____ a32c <= Another commit
        |___ file 'router.ts' created
```

**We:**
We are always in some branch, by default we always look at the latest commit we made with the new things we're writing (we can also travel through the commits of a branch, but for now we won't do that because it only complicates things and is rarely necessary to do so).


*Example*
```
        i18n a32c
---------|-----------------|-------------- master
        \
        |------------ development <= We
        c43k
```

In this last example, we see a new sub-branch called development. We are pointing to it, and we can return to master with a simple command if we want to (we will see this later).

This new sub-branch, at the moment it is created (yes, there is a command to do it), becomes independent of master, and the commits made in this new branch will not be on development. We can see it as if we were walking on a path called master, and suddenly we come across a fork, master continues to the left and on the right, we have a path called development. Everything that happens in development will not happen in master, and everything that happens in master (after the fork) will not happen in development.


### Git add

When we modify a file, Git knows that it has been modified, but it doesn't know if it should save that new modification in the next commit. Therefore, using the command `git add fileName`, we tell Git that in the next commit, the actions done on that file will be saved.

*Example*
```
A line is modified to the file modifie.js, and we want to save it on the next commit. 

> git add modifie.js
```

### Git commit

When we have already told git all the files we want to include, is time to make the commit. We just need to run `git commit -m "Sentence that describes the changes made"`.

*Example*
```
> git commit -m "A modification has been made to the treatment of tables on modified farms"
```

It is VERY important that commit messages are descriptive, indicating what is contained in that commit (so if we ever need to see a modification, we know which one to go to just by reading the message)

### Git status

This is probably one of the best commands in Git. When we don't remember which files we have modified or which files we have added, this command will provide all the necessary information.

*Example*
```
>git status

  On branch redactant_git
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

          modified: instructionsGit.md

  no changes added to commit (use "git add" and/or "git commit -a")
```

As we can see, the file instruccionsGit.md has been modified, but it has not been added yet.

### Git branch

Earlier we saw that from the master branch, the development sub-branch was created (if you haven't read it, please do). This branch was created while we were inside master, using the command `git branch development`.

*Example*
```
        i18n              a32c
---------|-----------------|-------------- master <= We
                           \ <= > git branch development
                            \ development
```

### Git checkout

When we have multiple branches, such as master, development, feature1, feature2, etc... we need to be able to switch between these branches somehow. To do this, we use `git checkout [branch name]`.

*Example*
```
        i18n              a32c
---------|-----------------|-------------- master <= We
                           \
                            |------------ development
                           c43k

> git checkout development

        i18n a32c
---------|-----------------|-------------- master
                            \
                             |------------ development <= We
                            c43k

```

### Git merge

This is the most difficult command in level 1. Normally, in the project, merge will only be done once each feature is completed, towards development. There is always a person responsible for merging all branches into development, therefore, using this command towards development without permission is prohibited.

When we have two different branches with different commits, we may be interested in combining the code from one branch with the code from the other. We can do this with the command `git merge [name of the branch we want to take]`.

*Example*
```
a32c i23s 3fs1
|-----|----|----- master <= We
\
 |------|---|---- development
u42a   n42m h21k
```

We want to make master branch gain the functionalities from development, therefore we will do `> git merge development`. ATTENTION if we do git merge master from the development branch, it will be development who gains the functionalities from master and not the other way around.

```
> git merge development
a32c i23s 3fs1
|-----|----|------|- master <= We
\                / > git merge development
 |------|---|---|--- development
u42a   n42m h21k
```

When we use this command, two things can happen. The first is that nothing happens and master has both functionalities, this is the best possibility. The problem arises if the commits between branches contain modifications to the same files, and here the second option appears: conflicts.

**Conflicts**

Whenever conflicts arise during a merge, Git notifies us with a message on the terminal, indicating which file has a conflict.

If we enter the file, we will have a structure like this:

*Example*
```
<<<<<<< HEAD
// Here is the code we have
=======
// Here is the new code
>>>>>>> Commit the branch that we want to keep
```

Here we have to decide which code we keep and which we don't, or which modifications need to be made according to which code. Therefore, we modify the file fixing the conflict and deleting the code that we don't want along with the messages that git has created.

## Part 2
In the previous level, the use of git was discussed by storing information locally on one's computer. The objective of this level is to explain the use of remote git, in an internet repository.

### Git clone
If we want to work on an internet repository, we must use the command `> git clone [http or ssh link provided by the repository]`.
An example would be `> git clone https://github.com/example/directory.git`
This command downloads the master branch, other branches are selected with other commands that we will see.

### Origin
When working with git, information can be saved locally or remotely (known as origin). Origin is the information connected with the internet repository.

Let's say that the information of the commits/project can be saved in three ways in total:

*Example*
```
Repository
---|-----|------|--
 j23k   l12j   k32g
__________
Origin
---|-----|--
  j23k  l12j
__________
Local
---|-----|----
 j23k  a34f
```

As we can see, we have the repository version, the origin version, and our version, the local one.
The repository version has three commits, the origin version has two of them, and finally, the local branch has one of them plus one that we have made ourselves, which no one else has.
We have many possibilities in this situation, we can update the origin, we can update the local without updating origin, we can update origin and local at the same time, etc...

### Git pull origin branch

When we are working locally and we want to download information from the repository, certain steps must be followed.

First of all, we must have a branch with the same name as the branch we want to download or update. Therefore, if we want to download the development branch and we haven't created it, we will have to do git branch development => git checkout development.

When we have a branch with the same name as the one in the repository, we can already use the command git pull origin [branch name]. This command will take the information from the repository and update Origin and Local (the branch from which we have executed the command).

Attention, if we have some commits in the local branch that are not in the repository and they touch the same part of the code as the commits we download, there will be conflicts that will need to be resolved.

*Example*
```
> git pull origin development
Repository
---|-----|------|-- <= development
  j23k  l12j   k32g
__________      \
Origin           \
---|-----|--------|- <= development
  j23k  l12j     k32g
_________\        \
Local     \        \
---|-----|-|--------|- <= development
 j23k  a34f        k32g
          l12j
```

### Git push origin branch

When our version (Local) of the project has all the commits from the repository and also has commits that we have made, we are interested in updating the repository. To do this, we will use the command git push origin [branch name]. This command takes all the commits we have in our branch and sends them to the remote repository, updating it.

*Example*
```
Repository                   a34f
---|-----|------|-------------|- <= development
  j23k  l12j   k32g          /
__________                  /
Origin                a34f /
---|-----|------|---------|- <= development
 j23k   l12j    k32g     /
__________              /
Local                  /
---|-----|------|-----|--- <= development
  j23k  l12j   k32g  a34f
```


We can also use git push -u origin [branch name] so that the next time we only need to use the git push command and Git will know where to push to.


## Part 3

In this section we will take a closer look at the commands that work with origin and the repository, as well as more complex commands.

### Git fetch
If we intend to update origin by downloading data from the repository, we need to update with git fetch.


*Example*
```
> git pull origin development
Repository
---|-----|------|-- <= development
 j23k   l12j   k32g
__________      \ <= > git fetch
Origin           \
---|-----|--------|- <= development
 j23k   l12j     k32g
```

### Git pull

If we want to update origin and update the branch we are working on, we will use git pull. This command is a combination of git fetch and git merge "current branch", that is:

*Example*
```
> git pull == > git fetch => > git merge

Repository
---|-----|------|-- <= development
  j23k  l12j   k32g
__________      \ <= git fetch
Origin           \
---|-----|--------|- <= development
  j23k  l12j     k32g
_________ \       \ <= git merge "Branch where we are"
Local      \       \
---|-----|-|--------|- <= development
 j23k   a34f       k32g
          l12j
```

What does this mean? This command is very useful but sometimes dangerous. If we are working on a branch and make modifications that are not in the repository, this command can cause conflicts (in the merge part).

### Git revert

This command is simple but very dangerous. If we have merged with the wrong branch or made an error that the previous commit did not have, this command is appropriate. However, if our goal is to go back to more than one commit, we must be very careful with this command, as it can cause many errors.

The command intends to make a new commit using a previous commit. What does this mean? We could say that it is a backup of previous information.

*Example*
```
--------|---------|-------|-- <= popUp
       g45o     i15g    a31o

> git revert i15g

                   ____________
                  /            \
--------|---------|-------|-----| <= popUp
       g45o     i15g     a31o  w32v (previous commit i15g)
```

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>
