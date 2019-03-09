# Cheat sheet

## create a local repository
Create an empty folder called *projetA*, and execute the following command in this folder:
```
git init
```
Now a .git folder is created in *projectA* and contains all git pieces of information about this freshly created local repository.

## commit
In the previously empty repo, fileA.txt is added

    git status

>On branch master  
No commits yet  
Untracked files:
  (use "git add \<file\>..." to include in what will be committed)  
fileA.txt  
nothing added to commit but untracked files present (use "git add" to track)

Current branch is the default branch called *master*.
No commits have been done so far but a new file is present in the working copy. However this file is not tracked yet.


    git add .\fileA.txt
    git status

>On branch master   
No commits yet  
Changes to be committed:  
  (use "git rm --cached \<file\>..." to unstage)  
        new file:   fileA.txt

Now fileA.txt is staged in the staging area and can be commited

    git commit -m'my first commit message'
>[master (root-commit) 213cf6c] my first commit message  
 1 file changed, 0 insertions(+), 0 deletions(-)  
 create mode 100644 fileA.txt  

The file is now commited, and a commit number is displayed (short SHA-1: *213cf6c*)

    git status
>On branch master  
nothing to commit, working tree clean

    git log
>commit 213cf6cb3a473c9c2305574926bb1103ffdf4922 (HEAD -> master)  
Author: XXX <XXX@mail.com>  
Date:   Fri Mar 8 18:51:03 2019 +0100  
    my first commit message

git log delivers information about the commit history. Here we can see the commit number (SHA-1). *HEAD* is is pointing on the branch label *Master*. *Master* is here the symbolic branch label pointing on the last commit (tip) of this branch. 
It is also possible to get a short log history on oneline with the branch graph:

    git log --oneline
> 213cf6c (HEAD -> master) my first commit message

Now fileA.txt is under version control. When this file is modified in the working copy, it has to be put in the staging area, and then commit it in order to have the modifications in the log history.

    git status
>On branch master
Changes not staged for commit:
        modified:   fileA.txt

    git add .\fileA.txt
    git status
>On branch master
Changes to be committed:
  (use "git reset HEAD \<file\>..." to unstage)
        modified:   fileA.txt

    git commit -m'featureA added'
>[master 54c09e1] featureA added
 1 file changed, 1 insertion(+)

Suppose that you now have a file *fileb.txt* created in the working copy. 
    
    git add .\fileb.txt
    git status
>On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   fileb.txt

PS D:\perso\courses\git course\hands on\repos\projecta> git rm --cached .\fileb.txt
rm 'fileb.txt'
PS D:\perso\courses\git course\hands on\repos\projecta> git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        fileb.txt

## Remote repository
### clone a remote repo
if no local repo is configured and a remote repo exists, the simplest way is to clone the remote repo that creates a local repo, manages the link between local and remote, copy the git history and create a working copy equivalent to the current branch. 

    git clone https://github.com/20centcroak/github-cheatsheet.git
The url is given by the remote repo manager (github, bitbucket, ...)
>Cloning into 'github-cheatsheet'...   
remote: Enumerating objects: 140, done.    
remote: Counting objects: 100% (140/140), done.  
remote: Compressing objects: 100% (136/136), done.  
Rremote: Total 140 (delta 40), reused 0 (delta 0), pack-reused 0                             0)  
Receiving objects: 100% (140/140), 18.63 KiB | 1.03 MiB/s, done.  
Resolving deltas: 100% (40/40), done.

A folder is created containing the .git foler and the working copy for the current branch

     cd .\github-cheatsheet\
     ls
 >    Répertoire : D:\perso\courses\git course\hands on\repos\croak\github-cheatsheet  

|Mode|LastWriteTime|Length|Name|
|--|--|--|--|
|-a----|09/03/2019 10:12|2626|cheatsheet.md|
|-a----|09/03/2019 10:12|287| README.md|

    git log --oneline
>fe2d511 (HEAD -> master, origin/master, origin/HEAD) cheatsheet.md updated from https://stackedit.io/  
a225ce5 cheatsheet.md updated from https://stackedit.io/  
3b1ef0d cheatsheet.md updated from https://stackedit.io/  
10bc7ba cheatsheet.md updated from https://stackedit.io/  
6fbb7ca cheatsheet.md updated from https://stackedit.io/  

Here we can see that HEAD points on the branch label *master*, the tracking branch *origin/master* and the symbolic remote link *origin/HEAD*
By default remote name is origin:  

    git remote
>origin

git remote shows identified remote repositories


### add a remote repo
When a local repo already exists and need to be identifed by a remote repo, we should declare this remote repository.
Suppose you have created a pseudo-remote repo in folder remotea (like  and you want to link remote repo *remotea* with local repo *projecta*:

    git remote add my-remote ..\remotea\   
    git remote
>my-remote

*my-remote* is the remote repo name we have assigned when pointing on remote repository. By default, this name is origin. 
It is possible to have more than one remote repository
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzQ4MjQ0NDE3LC0xMzI4MTk0MjgyLC0yMD
AyNzk5OTQ0LDMwMzUwNzQ3Myw0NzY5NzEwOCwxMzc2MTU0MjEs
LTExNTUzMzMwODAsLTUyODk0Njc3OSwxMzAyOTY4Njg1LDY3Mz
I5MzYxNSwxNzkyNTUxMDc3LC02MDUzMjk4ODMsMTQ4NjU1OTgz
OSw4ODg3MjAyODEsMTM0MDE5ODMyMSwtMTc1NDQ2ODA5NV19
-->