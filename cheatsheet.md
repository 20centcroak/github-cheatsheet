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
```
&nbsp;&nbsp;&nbsp;&nbsp;
```fileA.txt  
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

Now fileA.txt is staged in index and can be commited



<!--stackedit_data:
eyJwcm9wZXJ0aWVzIjoiZXh0ZW5zaW9uczpcbiAgcHJlc2V0Oi
B6ZXJvXG4iLCJoaXN0b3J5IjpbLTIzNzQwMTQ2MSwxMzQwMTk4
MzIxLC0xNzU0NDY4MDk1XX0=
-->