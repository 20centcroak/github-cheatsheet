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
  (use "git rm --cached <file>..." to unstage)  
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
Author: XXX <XXX.mail.com>  
Date:   Fri Mar 8 18:51:03 2019 +0100  
    my first commit message

git log delivers information about the commit history. Here we can see the commit number (SHA-1). *HEAD* is is pointing on the branch label *Master*. *Master* is here the symbolic branch label pointing on the last commit of this branch. 
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
 >

    git log --oneline
>fe2d511 (HEAD -> master, origin/master, origin/HEAD) cheatsheet.md updated from https://stackedit.io/  
a225ce5 cheatsheet.md updated from https://stackedit.io/  
3b1ef0d cheatsheet.md updated from https://stackedit.io/  
10bc7ba cheatsheet.md updated from https://stackedit.io/  
6fbb7ca cheatsheet.md updated from https://stackedit.io/  

<!--stackedit_data:
eyJwcm9wZXJ0aWVzIjoiZXh0ZW5zaW9uczpcbiAgcHJlc2V0Oi
B6ZXJvXG4iLCJoaXN0b3J5IjpbMTE3MTI3Nzk4OSwxNzkyNTUx
MDc3LC02MDUzMjk4ODMsMTQ4NjU1OTgzOSw4ODg3MjAyODEsMT
M0MDE5ODMyMSwtMTc1NDQ2ODA5NV19
-->