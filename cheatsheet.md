# Cheat sheet
## Structure and principle
[git workflow](https://blog.osteele.com/2008/05/my-git-workflow/)
![git workflow](https://images.osteele.com/2008/git-transport.png)
## Naming
* *workspace* or *working copy* is the folder containing the .git folder and all the files that will be examined by git if not declared as ignored
* *index* or *staging area* is where new files or modifications are added with the *add* command if we want them to be part of a new commit
* repositories are the version control containing base files and their associated modifications, and the commit history as well. The local repo is updated thanks to the *commit*, or *fetch* commands. The remote repo is updated thanks to the *push* command

## Create a local repository
Create an empty folder called *projectA*, and execute the following command in this folder:
```
git init
```
Now a .git folder is created in *projectA* and contains all git pieces of information about this freshly created local repository.

## Stage/unstage files
Suppose that you have a file *fileb.txt* created in the working copy. It can be added to the staging area:
    
    git add .\fileb.txt
    git status
>On branch master  
Changes to be committed:  
        new file:   fileb.txt  

The file has been added in the staging area. If you didn't mean to put it in the staging area, it can be removed with the following command:

    git reset HEAD .\fileb.txt
    git status
>On branch master  
Your branch is up to date with 'my_remote/master'.  
Untracked files:  
        fileb.txt  

The same command works to unstage any modifications. The file in the working copy is not modified and is still under version control (it is tracked), but the modifications are not staged for commit.

## Commit
In an empty repo, fileA.txt is added

    git status

>On branch master  
No commits yet  
Untracked files:
fileA.txt  
nothing added to commit but untracked files present

Current branch is the default branch called *master*.
No commits have been done so far but a new file is present in the working copy. However this file is not tracked yet. It has to be staged in the staging area and then commited.

    git add .\fileA.txt
    
Now *fileA.txt* is staged in the staging area and can be commited

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
    git commit -m'featureA added'
>[master 54c09e1] featureA added
 1 file changed, 1 insertion(+)

It can also be done with 1 command line

    git commit -a -m'featureA added'

This command add and commit all tracked and modified files.

## Information on history, commits and so on
We have already seen previously the *git status* command which look at the files in the working copy, in the staging area and in the version control and delivers a status for these files (created, modified, deleted, untracked, staged, ...)
    
    git status
>On branch master
Changes not staged for commit:
         modified:   fileA.txt
Untracked files:
          fileb.txt

git log delivers information about the commit history.
    git log
>commit 213cf6cb3a473c9c2305574926bb1103ffdf4922 (HEAD -> master)  
Author: XXX \<XXX@mail.com\>  
Date:   Fri Mar 8 18:51:03 2019 +0100  
    my first commit message

Here we can see the commit number (SHA-1). *HEAD* is is pointing on the branch label *Master*. *Master* is here the symbolic branch label pointing on the last commit (tip) of this branch. 
It is also possible to get a short log history on oneline with the branch graph:

    git log --oneline
> 213cf6c (HEAD -> master) my first commit message

In this short message, commit ID is shorten as well using only the 7 first characters of the SHA-1 ID.

Commit IDs (long or short)  can be used to get information about a specific commit. When not specified, the HEAD label is used to deliver informtation.

    git show
>commit 6a5dcf4779d2dc456ab460f080c9086c7697f13b (HEAD -> master, tag: v1.0, my_remote/master)
Author: XXX \<XXX@mail.com\>
Date:   Sat Mar 9 16:37:59 2019 +0100
    add feature2 in fileb

    git show 54c09e1
>commit 54c09e11bbaa05c9857f6589edc660e4be3c0798
Author: XXX\<XXX@mail.com\>
Date:   Fri Mar 8 19:06:28 2019 +0100
    featureA added

## Remote repository
### Clone a remote repo
if no local repo is configured and a remote repo exists, the simplest way is to clone the remote repo that creates a local repo, manages the link between local and remote, copy the git history and create a working copy equivalent to the current branch. 

    git clone https://github.com/20centcroak/github-cheatsheet.git
The url is given by the remote repo manager (github, bitbucket, ...)
>Cloning into 'github-cheatsheet'...   
remote: Enumerating objects: 140, done.    
remote: Counting objects: 100% (140/140), done.  
remote: Compressing objects: 100% (136/136), done.  
Rremote: Total 140 (delta 40), reused 0 (delta 0), pack-reused 0 (0)  
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

git remote shows identified remote repositories.

### Add a remote repo
When a local repo already exists and need to be identifed by a remote repo, we should declare this remote repository.
Suppose you have created a pseudo-remote repo in folder remotea:

    git init --bare
>Initialized empty Git repository in D:/repos/remoteb/

and you want to link remote repo *remoteb* with local repo *projecta*:

    git remote add my_remote ..\remoteb\
    git remote
>my_remote

*git remote* displays the declared repositories

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

It is possible to specify the remote branch associated to the local branch for push and pull, then there won't be any need to specify the remote repo and the branch when pushing/pulling:

    git push --set-upstream my_remote master
>Everything up-to-date

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

If we want our file to be deleted in our working copy too, the *--force* tag should be used instead of *--cached*
    
    git rm --force .\fileb.txt

## Tags
you can add a user-friendly reference to a specific commit to find it easily.

    git tag -a -m'my first tag' v1.0
*-a* or *-annotate* to build a true git object with date, author name and other metadata. *-m* is required when *-a* is used. It adds a comment. Eventually the tag name (*v1.0* here) should be mentioned as it becomes the reference.
You can review the list of tags  with this command. 

    git tag
>v1.0

you can delete a tag with the *-d* flag
 
    git tag -d v1.0
>Deleted tag 'v1.0' (was 3fc4051)

we can tag any commit knowing its ID (or a symbolic reference)

    git tag v1.0 -a -m'my new tag' d86540b
If no ID is supplied, then HEAD is used as the reference of the commit to tag

    git tag v1.1
    git tag
>v1.0
v1.1

    git show v1.0
>tag v1.0
Tagger: XXX\<XXX@mail.com\>
Date:   Sat Mar 9 18:56:22 2019 +0100
my new tag
commit d86540b0edf6dd05c1c30858db0c88ffca23ba88 (tag: v1.0)
    add feature in fileb

    git show v1.1
>commit 774c2a66d1ea30b6517c69c03e1911ede02686af (HEAD -> master, tag: v1.1)
Author: XXX\<XXX@mail.com\>
Date:   Sat Mar 9 17:20:44 2019 +0100
    delete fileb

Tags are not pushed to remote with the *git push* command. We must use the *--tags* flag
    
    git push --tags

## Branches
### Create branches
branch are created with 
    
    git branch featureX
then point to the created branch with 

    git checkout featureX
>Switched to branch 'featureX'

both can be done in 1 command
 
    git checkout -b featureY
>Switched to a new branch 'featureY'

### Delete branches
A branch can be deleted only if head is not pointing on it currently. If some works are
    git branch -d featureX
>Deleted branch featureX (was 774c2a6).

### Switch branch
You can switch from a branch to another  
    
    git checkout featureY

but you can't switch if there are uncommited changed for tracked files:
>error: Your local changes to the following files would be overwritten by checkout:
        fileb.txt
Please commit your changes or stash them before you switch branches.
Aborting

If you want to discard changes for a given file, use

    git checkout -- fileb.txt

Untracked files are noty affected when switching from a branch to another.

## References

Git uses commit IDs built with SHA-1 to reference a snaphsot, ie a given version of the files which has been declared thanks to the *git commit* command.
Besides, symbolic references are also used:
 * branch label (*master* for example) points on the latest commit (or tip) of a given branch, it has the name of the branch
 * HEAD points on a branch label and indicates the current state
 * HEAD~ or HEAD~1 points on the parent commit of HEAD
 * HEAD~~  or HEAD~2  points on the grand-parent commit of HEAD
 * remote branch label (*my_remote/master* for example) points on the latest commit of a remote branch
 * tracking branch (TODO)
 * tags are generated by users and points on specific commits

Let's have a look to a log:

    git log --oneline
    
>6cf4f3a (HEAD -> featureZ) new feature
da1bc46 new feature
774c2a6 (tag: v1.1, master, featureY) delete fileb
6a5dcf4 (my_remote/master) add feature2 in fileb
d86540b (tag: v1.0) add feature in fileb
acc534c delete fileA.txt
54c09e1 featureA added
213cf6c my first commit message

* HEAD indicates the current state: it points on the branch label *featureZ*, which itself points on the tip of this branch, which is the commit ID 6cf4f3a.
* tag v1.1, *master* branch label  and *featureY* branch label point on commit ID 774c2a6
* tag v1.0 points on commit d86540b

*fileb* has been deleted in commit 774c2a6, then it has been recreated and some content inside has been updated in commits da1bc46 and 6cf4f3a:
    
    git show da1bc46
>commit da1bc46f308edc8b4117973183fb4510d8327b69
Author: XXX\<XXX@mail.com\>
Date:   Sat Mar 9 19:19:09 2019 +0100
    new feature
diff --git a/fileb.txt b/fileb.txt
new file mode 100644
index 0000000..e72c7ed

Then if we switch from branch *featureZ* to branch *master*, we can see that our working copy is updated and *fileb* is present only in featureZ

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc5OTk2MTY5MSwxMjQ0MTE5NjUyLC03Nz
gwNTI5MjksLTE5NDIyNjAzNTIsLTE2NjMyMTExMDgsOTYwMTY1
NDE1LDQwNTYyMjc5MSwxMTEwODI4NTYsNDYyMjQ4NTI1LC0xOD
M5MDk1MjkxLC0zMzI5NDA4MzgsLTk2ODY3OTE0NCwyMDU0Njg4
MTQyLDExNDI2Mjk0NjAsLTE1MjYyOTE2MzUsOTk0Nzk5NDczLC
0xNDUwNDU4Mjg2LC0xNjczOTQ0MTQ5LDE0Nzc3OTQ1OTQsLTM2
MjE1MTM0OV19
-->