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

the file is now commited, and a commit number is displayed (short SHA-1: *213cf6c*)

    git status
>On branch master  
nothing to commit, working tree clean

    git log
>commit 213cf6cb3a473c9c2305574926bb1103ffdf4922 (HEAD -> master)  
Author: XXX <XXX.mail.com>  
Date:   Fri Mar 8 18:51:03 2019 +0100  
    my first commit message

git log delivers information about the commit history. Here we can see the 


<!--stackedit_data:
eyJwcm9wZXJ0aWVzIjoiZXh0ZW5zaW9uczpcbiAgcHJlc2V0Oi
B6ZXJvXG4iLCJoaXN0b3J5IjpbLTMwMTExODQ1NiwxMzQwMTk4
MzIxLC0xNzU0NDY4MDk1XX0=
-->