# Git guidelines to follow o Guidelines to follow for git

In this document you will be able to find git commands, accompanied by their use and explanation. These commands are grouped in three levels, as not all of them are as simple or easy to understand, nor as important. With the first two levels you will be able to work with the repository.

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
In this level, no detail is given about any of the explained concepts. Only a brief overview is provided to undestand what is being done.

### Git structure
In essence, Git is a tree. This tree is formed by two general concepts that someone must know like the back of one's own hand: Branches and commits.

**Commit:**
A commit is a version of our program, that we have saved.

*Example*
```
|____ i18n  <= A commit
|      |___ file 'pot.js' modified
|      |___ file 'index.html' deleted
|____ a32c  <= Another commit
       |___ file 'router.ts' created
```

**Branch:**
A branch is a chain of commits. In the previous case, it was the master branch. 

*Example*
```
master
|____ i18n  <= A commit
|      |___ file 'pot.js' modified
|      |___ file 'index.html' deleted
|____ a32c  <= Another commit
       |___ file 'router.ts' created
```

**We:**
We are always in some branch, by default we always look at the latest commit we made with the ner things we're writing (we can also travel through the commits of a branch, but for now we won't do that because it only complicates things and is rarely necessary to do so).

*Example*
```
        i18n              a32c
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

          modified:   instructionsGit.md

  no changes added to commit (use "git add" and/or "git commit -a")
```

As we can see, the file instruccionsGit.md has been modified, but it has not been added yet.

### Git branch

Earlier we saw that from the master branch, the development sub-branch was created (if you haven't read it, please do). This branch was created while we were inside master, using the command `git branch development`.

*Example*
```
        i18n              a32c
---------|-----------------|-------------- master  <= We
                            \ <=  > git branch development
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

        i18n              a32c
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
a32c  i23s 3fs1
|-----|----|----- master <= We
\
 |------|---|---- development
u42a   n42m h21k
```

We want to make master branch gain the functionalities from development, therefore we will do `> git merge development`. ATTENTION if we do git merge master from the development branch, it will be development who gains the functionalities from master and not the other way around.

```
> git merge development
a32c  i23s 3fs1
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

Here we have to decide which code we keep and which we don't, or which modifications need to be made according to which code. Therefore, we modify the file fixing the conflict and delete the code that we don't want along with the messages that git has created.

## Part 2
En el nivell anterior s'ha comentat l'ús de git guardant la informació localment, en l'ordinador propi. L'objectiu d'aquest nivell és explicar l'ús de git remot, a un repositori d'internet.

### Git clone
Si volem treballar sobre un repositori d'internet, hem d'utilitzar la comanda >git clone [enllaç http o ssh que ens proporciona el repositori].
Un exemple seria > git clone https://github.com/exemple/directori.git
Aquesta comanda descarrega la branca master, les altres branques s'elegeix amb altres comandes que veurem.

### Origin
Quan es treballa amb git es pot guardar informació a local o a remot (conegut com origin). Origin és la informació connectada amb el repositori d'internet.

Diguem que la informació dels commits/projecte, en total, es pot guardar de tres maneres:

*Exemple*
```
Repositori
---|-----|------|--
 j23k   l12j   k32g
__________
Origin
---|-----|--
 j23k   l12j
__________
Local
---|-----|----
 j23k  a34f
```

Com podem veure, tenim la versió del repositori, la versió d'origin, i la nostra versió, la local.
La versió del repositori té tres commits, la versió d'origin en té dos d'ells, i finalment la branca local en té un d'ells més un que hem fet nosaltres, que no té ningú més.
Tenim moltes possibilitats en aquesta situació, podem actualitzar l'origin, podem actualitzar el local sense actualitzar origin, podem actualitzar origin i local a la vegada, etc...

### Git pull origin branch

Quan nosaltres estem en local i volem descarregar informació del repositori, s'han de seguir uns passos.

Primer de tot, s'ha de tenir una branca amb el mateix nom que la branca que volem descarregar o actualitzar. Per tant, si volem descarregar la branca development i no la tenim creada, haurem de fer git branch development => git checkout development.

Quan tenim una branca amb el mateix nom que al repositori, ja podem utilitzar la comanda git pull origin [nom de la branca]. Aquesta comanda agafarà la informació del repositori i actualitzarà Origin i Local (la branca des de la que hem executat la comanda).

Atenció, si tenim alguns commits en la branca local, que no té el repositori, i toquen la mateixa part de codi que els commits que descarreguem, hi haurà conflictes, s'hauran de solucionar.

*Exemple*
```
> git pull origin development
Repositori
---|-----|------|-- <= development
 j23k   l12j   k32g
__________      \
Origin           \
---|-----|--------|- <= development
 j23k   l12j     k32g
_________\        \
Local     \        \
---|-----|-|--------|- <= development
 j23k  a34f        k32g
          l12j
```

### Git push origin branch

Quan la nostra versió (Local) del projecte té tots els commits del repositori, i a més a més té commits que hem fet nostaltres, ens interessa actualitzar el repositori. Per fer-ho, utilitzarem la comanda git push origin [nom de la branca]. Aquesta comanda agafa tots els commits que tenim en la nostra branca, i els envia al repositori remot, actualitzant-ho.

*Exemple*
```
Repositori                   a34f
---|-----|------|-------------|- <= development
 j23k   l12j   k32g          /
__________                  /
Origin                a34f /
---|-----|------|---------|- <= development
 j23k   l12j   k32g      /
__________              /
Local                  /
---|-----|------|-----|--- <= development
 j23k   l12j   k32g  a34f
```

També podem fer git push -u origin [nom branca], aixi les següents vegades només fara falta utilitzar la comanda git push i git ja sabrà a quina localització fer push.

## Part 3

En aquest apartat veurem més a fons les comandes que treballen sobre origin i el repositori, a part de comandes més complexes.

### Git fetch
Si la nostra intenció és actualitzar l'origin descarregant les dades del repositori, hem d'actualitzar git fetch.

*Exemple*
```
> git pull origin development
Repositori
---|-----|------|-- <= development
 j23k   l12j   k32g
__________      \   <= >git fetch
Origin           \
---|-----|--------|- <= development
 j23k   l12j     k32g
```

### Git pull

Si el que volem és actualitzar l'origin i actualitzar la branca en la que treballem, utilitzarem git pull. Aquesta comanda és una convinació de git fetch i git merge "branca en la que estem", és a dir:

*Exemple*
```
>git pull == >git fetch => >git merge

Repositori
---|-----|------|-- <= development
 j23k   l12j   k32g
__________      \   <= git fetch
Origin           \
---|-----|--------|- <= development
 j23k   l12j     k32g
_________\        \ <= git merge "branca en la que estem"
Local     \        \
---|-----|-|--------|- <= development
 j23k  a34f        k32g
          l12j
```

Que significa això? Aquesta comanda es molt util, però a vegades perillosa. Si estem treballant sobre una branca i fem modificacions que no té el repositori, aquesta comanda pot ocasionar conflictes (en la part de merge).

### Git revert

Aquesta comanda es simple, però molt perillosa. Si hem fet un merge amb una branca equivocada, o un error que el commit anterior no tenia, aquesta comanda es l'indicada, però si el nostre objectiu es tirar més d'un commit enrere, s'ha de vigilar molt amb aquesta comanda, ja que pot provocar molts errors.

L'intencio de la comanda és fer un nou commit amb l'ús d'un commit anterior. Que vol dir això? Podriem dir que es un backup d'informació anterior.

*Exemple*
```
--------|---------|-------|-- <= popUp
      g45o      i15g    a31o

> git revert i15g

                   ____________
                  /            \
--------|---------|-------|-----| <= popUp
      g45o      i15g    a31o   w32v (anterior commit i15g)
```

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>
