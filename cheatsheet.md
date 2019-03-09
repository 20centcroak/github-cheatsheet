# Cheat sheet
## Structure and principle
[git workflow](https://blog.osteele.com/2008/05/my-git-workflow/)
![git workflow](https://images.osteele.com/2008/git-transport.png)

## Create a local repository
Create an empty folder called *projetA*, and execute the following command in this folder:
```
git init
```
Now a .git folder is created in *projectA* and contains all git pieces of information about this freshly created local repository.

## Commit
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

### Commit modifications
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
 Stage/unstage files

Suppose that you now have a file *fileb.txt* created in the working copy. 
    
    git add .\fileb.txt
    git status
>On branch master
Changes to be committed:
        new file:   fileb.txt

The file has been added in the staging area. If you didn't mean to put it in the staging area, it can be removed with the following command:

    git rm --cached .\fileb.txt
>rm 'fileb.txt'

    git status
>On branch master
Untracked files:
        fileb.txt

If modifications occured on fileA.txt and the file is added to the staging area, then it is possible to commit the changes or to reset the changes of this fles. Then they won't be taken into account in the next commit.

    git status
>On branch master
Changes not staged for commit:
        modified:   fileA.txt
Untracked files:
fileb.txt

    git add .\fileA.txt
    git status
>On branch master
Changes to be committed:
         modified:   fileA.txt
Untracked files:
        fileb.txt

    git reset HEAD .\fileA.txt
>Unstaged changes after reset:
M       fileA.txt

    git status
>On branch master
Changes not staged for commit:
        modified:   fileA.txt
Untracked files:
fileb.txt

As we can see *fileA.txt* is still under version control (it is tracked), but the modifications are not staged for commit. The content of the file is not modified by this call to git reset.

If we don't want to track *fileA.txt* anymore, then we should call git rm:

     git rm --cached .\fileA.txt
>rm 'fileA.txt'
 
    git status
>On branch master
Changes to be committed:
        deleted:    fileA.txt
Untracked files:
        fileA.txt
        fileb.txt

This action can be aborted as any other modification in the staging area:

    git reset HEAD .\fileA.txt
>Unstaged changes after reset:
M       fileA.txt

    git status
>On branch master
Changes not staged for commit:
         modified:   fileA.txt
Untracked files:
          fileb.txt

## Information on history, commits and so on
We have already seen previously the *git status* command which look at the files in the working copy, in the staging area and in the version control
    
    git status
>On branch master
Changes not staged for commit:
         modified:   fileA.txt
Untracked files:
          fileb.txt

git log delivers information about the commit history.
    git log
>commit 213cf6cb3a473c9c2305574926bb1103ffdf4922 (HEAD -> master)  
Author: XXX <XXX@mail.com>  
Date:   Fri Mar 8 18:51:03 2019 +0100  
    my first commit message

Here we can see the commit number (SHA-1). *HEAD* is is pointing on the branch label *Master*. *Master* is here the symbolic branch label pointing on the last commit (tip) of this branch. 
It is also possible to get a short log history on oneline with the branch graph:

    git log --oneline
> 213cf6c (HEAD -> master) my first commit message


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
Suppose you have created a pseudo-remote repo in folder remotea:

    git init --bare
>Initialized empty Git repository in D:/repos/remoteb/

and you want to link remote repo *remoteb* with local repo *projecta*:

    git remote add my_remote ..\remoteb\
    git remote
>my_remote

*my_remote* is the remote repo name we have assigned when pointing on remote repository. By default, this name is origin. 
It is possible to have more than one remote repository

## Push commits to remote repository

    git push my_remote master
>Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (6/6), 442 bytes | 110.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To ..\remoteb\
 \* [new branch]      master -> master

The *master* branch has been created in the remote repo.
Just after this push, if we look at the log of the remote repo and the local repo, they are exactly the same:
    
    git log --oneline my_remote/master
>54c09e1 (HEAD -> master, my_remote/master) featureA added
213cf6c my first commit message

    git log --oneline master
>54c09e1 (HEAD -> master, my_remote/master) featureA added
213cf6c my first commit message

However, locally *fileb.txt* has been added and *fileA.txt* has been changed but the modifications have not been staged, then the status of remote and local repos are not the same:

    git status
>On branch master
Changes not staged for commit:
        modified:   fileA.txt
Untracked files:
        fileb.txt

    git status my_remote/master
>On branch master
nothing to commit, working tree clean

It is possible to specify the remote branch associated to the local branch for push and pull, then there won't be any need to specify the remote repo and the branch when pushing/pulling:

    git push --set-upstream my_remote master
>Everything up-to-date

## Deleting a file
In the previous state we have explored, our remote repo is in synch with our local repo, our working copy contains *fileA.txt* with modifications that are not staged and *fileb.txt* which is not under version control.
Suppose you want to remove *fileA.txt* from version control:

    git rm --cached .\fileA.txt
>rm 'fileA.txt'

    git status
>On branch master
Your branch is up to date with 'my_remote/master'.
Changes to be committed:
        deleted:    fileA.txt
Untracked files:
        fileA.txt
        fileb.txt

    git commit -m 'delete fileA.txt'
>[master acc534c] delete fileA.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 fileA.txt

    git push
>Counting objects: 2, done.
Writing objects: 100% (2/2), 201 bytes | 100.00 KiB/s, done.
Total 2 (delta 0), reused 0 (delta 0)
To ..\remoteb\
   54c09e1..acc534c  master -> master

Now our working copy still contains *fileA.txt* and *fileb.txt* but *fileA.txt* is not tracked anymore and if we look at the files in the  *master* branch of our remote repo, *fileA.txt* has been deleted.
The log shows the deletion:

    git log --oneline
>acc534c (HEAD -> master, my_remote/master) delete fileA.txt
54c09e1 featureA added
213cf6c my first commit message
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTAyNjczMDcxLDE0Nzc3OTQ1OTQsLTM2Mj
E1MTM0OSwxMjIwNTE3NjEyLDE0NTY5MDkyOTgsLTEzODU1Njc0
MzMsMTgyODY3Njg3MSwtNTk1MTkxNDY0LC0xMzI4MTk0MjgyLC
0yMDAyNzk5OTQ0LDMwMzUwNzQ3Myw0NzY5NzEwOCwxMzc2MTU0
MjEsLTExNTUzMzMwODAsLTUyODk0Njc3OSwxMzAyOTY4Njg1LD
Y3MzI5MzYxNSwxNzkyNTUxMDc3LC02MDUzMjk4ODMsMTQ4NjU1
OTgzOV19
-->