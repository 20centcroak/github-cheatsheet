# Cheat sheet
## Structure and principle
[git workflow](https://blog.osteele.com/2008/05/my-git-workflow/)
![git workflow](https://images.osteele.com/2008/git-transport.png)
## Naming
* *workspace* or *working copy* is the folder containing the .git folder and all the files that will be examined by git if not declared as ignored
* *index* or *staging area* is where new files or modifications are added with the *add* command if we want them to be part of a new commit
* *repositories* are the version control systems containing base files and their associated modifications, and the commit history as well. The local repo is updated thanks to the *commit*, or *fetch* commands. The remote repo is updated thanks to the *push* command
* *hunk* is a part of a file
* *branch tip* is the last commit of a branch

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

Note that if the commit message flagged with *-m* is not set, the text editor set in git config will prompt to add the commit message which is required.

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
A branch is a set of commit that trace back to the project's first commit.
### Create branches
branch are created with 
    
    git branch featureX
*HEAD* is still pointing on the current branch (*master* for example).  If we want to work in the *featureX* branch, we should point to the created branch with 

    git checkout featureX
>Switched to branch 'featureX'

Now *HEAD* points on *featureX* branch label.

both can be done in 1 command:
 
    git checkout -b featureY
>Switched to a new branch 'featureY'

The branch is created and *HEAD* points on *featureX* branch label.

### Delete branches
A branch can be deleted only if head is not pointing on it currently. 
    git branch -d featureX
>Deleted branch featureX (was 774c2a6).

Actually it only deletes the branch label. This is fine while there is no modification in this branch that has not been merged in another branch. If not, an error message is displayed because the modifications can't be referenced by any other git reference (commit, branch label, tags, ...) and would be lost.

    git branch -d featureFromFirstCommit
>error: The branch 'featureFromFirstCommit' is not fully merged.  

To delete the branch and all the works that have been done on it and not merged, use the -D flag

    git branch -D featureFromFirstCommit
>Deleted branch featureFromFirstCommit (was a0e0982).

If regrets arose after this deletion it is possible to recreate a branch with the commit history. However, git manages a garbage process and we should ensure that these commits are still present in the object store. If not, commits are **definitely lost**. 
To check that,  the only thing is to know the commit ID of the last commit of this branch and see if it present in the reflog which indicates the latest local changes:

    git reflog
>6cf4f3a (HEAD -> featureZ) HEAD@{0}: checkout: moving from featureFromFirstCommit to featureZ  
**a0e0982 HEAD@{1}: commit: add new content**  
dce055a HEAD@{2}: checkout: moving from featureZ to featureFromFirstCommit  
6cf4f3a (HEAD -> featureZ) HEAD@{3}: checkout: moving from featureFromFirstCommit to featureZ  
dce055a HEAD@{4}: commit: add feature in fileA from first commit

then, as done when creating a branch pointing on a specific, we can recreate a branch:

    git checkout -b featureFromFirstCommitRetrieved a0e0982
>Switched to a new branch 'featureFromFirstCommitRetrieved'

    git log --oneline
>a0e0982 (HEAD -> featureFromFirstCommitRetrieved) add new content  
dce055a add feature in fileA from first commit  
213cf6c my first commit message

### List branches
list local branches only:

    git branch
>\* (HEAD detached at 54c09e1)  
  featureY  
  featureZ  
  master

The asterisk shows the current branch
list local and remote branches:

    git branch -a
>\* (HEAD detached at 54c09e1)  
  featureY  
  featureZ  
  master  
  remotes/my_remote/featureZ  
  remotes/my_remote/master

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

### Create a branch from a commit
It is possible to retrieve a specific version of the files by positionning HEAD on a given commit

    git checkout 213cf6c
>Note: checking out '213cf6c'.   
You are in 'detached HEAD' state. You can look around, make experimental changes and commit them, and you can discard any commits you make in this state without impacting any branches by performing another checkout.  
If you want to create a new branch to retain commits you create, you may do so (now or later) by using -b with the checkout command again. Example:  
  git checkout -b \<new-branch-name\>  
HEAD is now at 213cf6c... my first commit message

*Head* is said "detached", meaning that it does not point on a branch lable but directly on a commit. It is fine to review files in this sepecific version, but if we want to work on these files, we should create a branch. Following the given instruction, create a branch from this state:
 
    git checkout -b featureFromFirstCommit
>Switched to a new branch 'featureFromFirstCommit'

Both command can be combined: 

    git checkout -b featureFromFirstCommit 213cf6c
>Switched to a new branch 'featureFromFirstCommit'

## References

Git uses commit IDs built with SHA-1 to reference a snaphsot, ie a given version of the files which has been declared thanks to the *git commit* command.
Besides, symbolic references are also used:
 * branch label (*master* for example) points on the latest commit (or tip) of a given branch, it has the name of the branch
 * HEAD indicates the current state and generally points on a branch label. In some cases, *HEAD* is said "*detached*", meaning that it does not point on a branch label but directly on a commit. This is the case when using the command `git checkout <commit>`
 * HEAD~ or HEAD~1 points on the parent commit of HEAD
 * HEAD~~  or HEAD~2  points on the grand-parent commit of HEAD
 * remote branch label (*my_remote/master* for example) points on the latest commit of a remote branch
 * tracking branch (TODO)
 * tags are generated by users and points on specific commits

## Understanding a log
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

branch *featureZ*:
    
    ls
> Répertoire : D:\perso\courses\git course\hands on\repos\projecta

|Mode|LastWriteTime|Length|Name
|--|--|--|--|
|-a----|09/03/2019     12:01|18|fileA.txt|
|-a----|10/03/2019     10:31|22|fileb.txt|

branch *master*:
    
    ls
> Répertoire : D:\perso\courses\git course\hands on\repos\projecta

|Mode|                LastWriteTime|         Length| Name|
|--|--|--|--|
|-a----|       09/03/2019     12:01|             18| fileA.txt|

## Merge
Merge put together works from 2 different branches.

### Fast-forward
Fast forward merge simply moves the branch label to the latest commit. It occurs when a new feature branch has been created from a base branch, commits are done on the feature branch but no modification is done on the base branch, then moving the branch label of the master branch to the last commit of the feature branch integrates the feature works to the base branch with no other needed operation.

### Merge commit
If merged can't be fast-foward (moving the branch label is not good enough to merge works since commits have been done in feature and bese branch for example), a merge commit will create a new commit putting together the commits of base and feature branches. This commit point on latest feature commit and latest base commit.
Even if fast-forward is possible, we can require a merge commit with a specific flag.

### Case study - fast forward merge

Let's say we have created a file *fileA* in the base branch called master. Then we have created a new branch called *develop* and work in it. In a first commit we have modified *fileA* and in a second commit we have created *fileB*.
No works have been done on *master* branch, then fast-forward merge is possible.

    echo 'create file' > fileA.txt
    git add .\fileA.txt
    git commit -m 'create fileA'
>[master (root-commit) f604538] create fileA  
 1 file changed, 0 insertions(+), 0 deletions(-)  
 create mode 100644 fileA.txt  

    git branch
>\* master

    git checkout -b develop
>Switched to a new branch 'develop'

    echo 'feature1' >> .\fileA.txt
    git commit -a
>[develop ceb9c6f] feature1 added  
 1 file changed, 0 insertions(+), 0 deletions(-)  

    echo 'create fileB' > fileB.txt
    git add .\fileB.txt
    git commit -m'add fileB'
>[develop 9f79a2a] add fileB  
 1 file changed, 0 insertions(+), 0 deletions(-)  
 create mode 100644 fileB.txt  

Before merge, branch *develop*:
 
    git log --oneline --graph
>\* 9f79a2a (HEAD -> develop, origin/develop) add fileB  
>\* ceb9c6f feature1 added  
>\* f604538 (origin/master, master) create fileA

Before merge, branch *master*:

    git log --oneline
>f604538 (HEAD -> master, origin/master) create fileA

Then, merge *develop* commits in *master* branch:

    git merge develop
>Updating f604538..9f79a2a
**Fast-forward**  
 fileA.txt | Bin 28 -> 48 bytes  
 fileB.txt | Bin 0 -> 30 bytes  
 2 files changed, 0 insertions(+), 0 deletions(-)  
 create mode 100644 fileB.txt  

We can observe that this is a fast-forward merge, then the *master* branch label is moved to the last commit. The log shows a linear branch with no reference to the fork to *develop*:

     git log --oneline --graph
>\* 9f79a2a (HEAD -> master, origin/develop, develop) add fileB  
\* ceb9c6f feature1 added  
\* f604538 (origin/master) create fileA

Finally, *HEAD* points to the symbolic references *master* and *develop* which points the last commit. 
*origin/develop* mention shows that the remote branch *origin/develop* also points this last commit because a *push* command has been called on this last commit for this remote branch. 
*origin/master* mention shows that the remote branch *origin/master* points on the first commit because no push have been applied to the master branch since this first commit.

### Case study - merge commit
Starting from the previous case study, let's create a branch *featureX*:

    git checkout -b "featureX"
>Switched to a new branch 'featureX'

add 2 commits in this branch with successive modifications of files A and B:

    echo 'featureX' >> .\fileA.txt
    git commit -a
>[featureX 9e469f9] add featureX in fileA  
 1 file changed, 0 insertions(+), 0 deletions(-)
    
    echo 'featureXb' >> .\fileB.txt
    git commit -a -m'add featureX in fileB'
>[featureX 3c7f318] add featureX in fileB  
 1 file changed, 0 insertions(+), 0 deletions(-)
 
 Then swith to *master* to merge *featureX* in it:
 
    git checkout master
>Switched to branch 'master'  
Your branch is ahead of 'origin/master' by 2 commits.  
  (use "git push" to publish your local commits)
    
  Use flag *--no-ff* to forbid fast-forward merge. Then A commit is generated after the last commit (*3c7f318*).
  
    git merge --no-ff featureX
>Merge made by the 'recursive' strategy.  
 fileA.txt | Bin 48 -> 68 bytes  
 fileB.txt | Bin 30 -> 52 bytes  
 2 files changed, 0 insertions(+), 0 deletions(-)  

Have a look to the log and graph:

    git log --oneline --graph
>\*   8fb47dd (HEAD -> master) Merge branch 'featureX'  
|\\  
| * 3c7f318 (featureX) add featureX in fileB  
| * 9e469f9 add featureX in fileA  
|/  
\* 9f79a2a (origin/develop, develop) add fileB  
\* ceb9c6f feature1 added  
\* f604538 (origin/master) create fileA  

This time we can see the fork in the graph with our 2 commits done in the featureX branch and then a merge with a generated commit (*8fb47dd*).
Finally *HEAD* points to the *master* branch label which points to the last commit. Last commit and previous commit are identical in this case.

We can also see that no commit have been added in the *master branch between the fork and the merge.
Note that *develop* branch label and the tracking branch *origin/develop* points on the commit before the fork. 

Let's delete references to the develop local and remote branches:

    git branch -d develop
>Deleted branch develop (was 9f79a2a).

and the remote branch in our remote repo called *origin*

    git push origin --delete develop
>To ..\remote\  
 \- [deleted]         develop

### Case study - commits in 2 different branches

    echo 'master modif' >> .\fileA.txt
    git commit -a -m'master modif'
>[master b98c8a2] master modif  
 1 file changed, 0 insertions(+), 0 deletions(-)

    git checkout -b featureY 8fb47dd
>Switched to a new branch 'featureY'

    echo 'featureY' >> .\fileB.txt
    git commit -a -m'add featureY'
>[featureY 3d060c3] add featureY  
 1 file changed, 0 insertions(+), 0 deletions(-)

    git checkout master
>Switched to branch 'master'  

    git merge featureY
>Merge made by the 'recursive' strategy.  
 fileB.txt | Bin 52 -> 72 bytes  
 1 file changed, 0 insertions(+), 0 deletions(-)

    git log --oneline --graph
>\*   e936a18 (HEAD -> master) Merge branch 'featureY'  
\|\\  
\| \* 3d060c3 (featureY) add featureY  
\* | b98c8a2 master modif  
\|/  
\*   8fb47dd Merge branch 'featureX'  
\|\\  
\| \* 3c7f318 (featureX) add featureX in fileB  
\| \* 9e469f9 add featureX in fileA  
\|/  
\* 9f79a2a add fileB  
\* ceb9c6f feature1 added  
\* f604538 (origin/master) create fileA  

We can see that there has been a modification in the *master* branch and another in the *featureY* branch. Then a commit has been generated to merge these 2 commits.


### Conflicts
When the same hunks of the same files have been modified in the 2 branches we want to merge, git identifies a conflict that should be solved by the user before merging the branches.

    git checkout -b conflict-feature
>Switched to a new branch 'conflict-feature'

    echo "conflict" >> .\fileA.txt
    git commit -a -m"modify hunk to get conflict"
>[conflict-feature 943fb20] modify hunk to get conflict  
 1 file changed, 0 insertions(+), 0 deletions(-)

    git checkout master
>Switched to branch 'master'  

    echo "conflict-master" >> .\fileA.txt
    git commit -a -m"conflict in master"
>[master b9234fd] conflict in master  
 1 file changed, 0 insertions(+), 0 deletions(-)

    git log --oneline --graph --all
>\* b9234fd (HEAD -> master) conflict in master
| * 943fb20 (conflict-feature) modify hunk to get conflict
|/
\* 1edbecb (featureZ) adding featureZ
\*   e936a18 Merge branch 'featureY'

    git merge conflict-feature
>Auto-merging fileA.txt  
CONFLICT (content): Merge conflict in fileA.txt  
Automatic merge failed; fix conflicts and then commit the result.  

    git status
>On branch master  
You have unmerged paths.  
  (fix conflicts and run "git commit")  
  (use "git merge --abort" to abort the merge)  
Unmerged paths:  
  (use "git add \<file\>..." to mark resolution)  
        both modified:   fileA.txt  
no changes added to commit (use "git add" and/or "git commit -a")

Merge should be aborted or conflicts should be solved by reviewing the concerned files.

#### Merge abort
Here is the command to cancel a merge request when conflict arose when processing this merge:

    git merge --abort

### Fix conflicts
Once the file has been manually corrected:
    
    git add .\fileA.txt
    git commit
>[master 79930f9] Merge branch 'conflict-feature'

## Tracking branch
a tracking branch is a reference to a remote branch. They are associated with a local branch. Generally local and remote branch share their name but this is the user's choice.

Let's create and chekout a new branch:

    git checkout -b develop
>Switched to a new branch 'develop'  

Then create a remote branch named *develop* in the remote repo *origin*. As HEAD is positionned on the local *develop* branch The *set-upstream* command 
    git push --set-upstream origin develop
Total 0 (delta 0), reused 0 (delta 0)
To ..\remote\
 * [new branch]      develop -> develop
Branch 'develop' set up to track remote branch 'develop' from 'origin'.
PS D:\perso\courses\git course\hands on\repos\project> git log --oneline
79930f9 (HEAD -> develop, origin/master, origin/develop, master) Merge branch 'conflict-feature'
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2Mzg5NTg2MTQsMTg5NzY3MzM1NiwtMT
Y4Mzk1MDUzNCwxNTcyNDk0NDg2LC01ODA3MTI1MTgsMTc3ODg1
MDQwNSwtNzgzNjIyMTA1LDMyNTA3NTI2Myw5MDYzMTgwMTUsLT
E2ODY5NjY2OTEsLTIwMDQ0OTk0MTksMTk1MTcwMTQzMywxNDQx
NjY2MzcwLDE1OTEzOTM0OTksLTE2NDI2MDgxOTQsLTE1MTMzMD
I4MiwxMTU5MzYzNTg3LDE5OTY0Njc3NzksMzU5NjQ1MDc2LC01
OTA1NDM2MjZdfQ==
-->