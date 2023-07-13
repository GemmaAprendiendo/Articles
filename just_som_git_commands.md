#Just Some Git Commands

```
Working Directory (folder on your computer) 
|
|
----------git add somefile (put it in the staging area)
            |
            |
            ------------git commit (all the staged file on the same commit)
                        |
                        |
                        ---The changes are part of the local repo, now you can 
                            |
                            |
                            git push origin somepathintheremoterepo

```

main/master are the branches that should always be ok, you don't work on those

Creating a new branch doesn't create a new folder

Switching branches when you have unstaged changes will either give an error or it will merge to the new branch, but you don't want that.  Don't do that.

Always check your status before doing things , with git status.

## GIT (GitHub following this one)

```
git config --global user.name - check your username

git config --global user.name "theusername" - set the user name

git config --global user.email - similar for email

git config --global user.email theemail@whatever.com

git --version

start . - popup windows explorer (open if mac)

ls - list contents

ls someFolderName

clear

ls someFolder/SomeFolderInsideThatOne

ls -a - all, including hidden files

pwd - present working dir

cd SomeFolder/ - change dir

cd .. - back up one

touch someFolder/somefile.txt - create file

mkdir someFolder

rm somefile.txt - remove

rm -rf someFolder - remove for dir

git status - check status of repository

git init - create a new repository, don't do this inside of an existing repository

git add file1 file2 - stage the changes so they can be committed

git add . - stage all changes

git commit

git commit -m "my message"

git log - see commits so far

git commit --amend - add changes to previous commit, opens prev commit msg (configure 

for your editor if needed)

git log --oneline - commits in just one line

git branch - see your branches (* for current one)

git branch -v - same but more info

git branch someBranchName - create branch, but stay current where you were current

git switch someBranchName - now current is that branch (HEAD points there now)

git switch -c someBranchName - create branch and go to it

git config core.editor notepad

git config --global core.editor "code --wait"

git start someFileName.txt - open the file in the version the current branch has of it.

git branch -d branchToDelete Not current branch, must be fully merged

git branch -D branchToDelete doesn't have to be fully merged

git branch -m newNameForBranch Changed the current branch's name

git merge TheBranchYouWantToMerge merge that branch to the current branch, where you type that.

git diff differences between staging area and working dir, any number of files.

git diff HEAD changes in working dir since last commit (staged or not).

git diff --staged Show staged changes (diff between last commit and staging area)

git diff --staged somefilename.txt also with the other options, can choose for what file.

git diff branch1 branch2 can also do git diff branch1..branch2 to compare branches, all files. Order matters.

git diff XXXXX..YYYYY XXXXX and YYYYY being some commit numbers that you found out with 

git log.

git stash stash uncommitted so they don't follow you to another branch when you switch

git stash save same thing

git stash pop remove most recent stash and put it back in working dir or staging area in branch

git stash apply apply from stach but the stach is still there, not removed as with pop 
(if you want to reapply to other branches)

git stash -u include untracked files

git stash list

git stash apply stash@{2} spefic stash, see with the list.

git stash drop stash@{2}

git stash clear

git checkout specificCommtHash - view that previous commit. HEAD points to the specific commit, not the branch

git switch someBranchName - HEAD back pointing to the branch

git checkout specificCommtHash git switch -c newbranch from commit in the past, create a new branch, from that point.

git checkout HEAD~2 - go to commits in the past

git checkout HEAD somefilename - discard those changes, back to when we last committed. Be mindful of where HEAD is pointing to.

git checkout -- somefilename - as above

git restore somefilename - as above

git restore --source HEAD~2 app.js - restore app.js to its state 2 commits before the 
one where HEAD is pointing now.

git restore --staged someFileName - remove from staging area

git reset someCommitHashTOGOBACKTO - undo commits and go back to that commit.

git reset --hard HEAD~2 - undo commits and file changes to that point (instead of HEAD~2, can also give the someCommitHashTOGOBACKTO)

git revert someCommitHashIWantTOGetRidOf - undo commits by committing a change that undoes to changes.

git tag tagname label to a moment in time, will put it in the HEAD

git tag -a tagnamemore detailed tag

git tag -a tagname commithash

git tag -d tagname delete

git push --tags push all tags

git tag see existing tags

git tag -l "*beta*" tags that have beta in the name

git checkout 15.3.1 check particular tag

git diff v15 v16

git tag -f tagnamethatwearealradyusing use -f to be able to have a duplicate tag

```

## GitHub or similar

```
git clone someurl Don't do this inside an existing repo, it will create the repo in the folder where you are

git remote check if local repo is already connected to github

git remote -v same but more details

git remote add origin someurl to establish the connection, origin can be another name, 
but this is the standard.

git remote rename oldname newname

git remote remove thename

git push originOrWhateverRemoteName masterOrWhateverBranchNameWeSendUp On GitHub, it will go to the same branch name that you use in the command.

git push origin myLocalBranch: toThisRemoteBranch branches with different names in local and remote.

git push -u origin myBranch configure upstream so doing "git push" already knows what to do

git branch -r Check what the branches are in remote (origin)

git checkout origin/master check out that branch, master or another one , how was it last time we communicated with it.

git switch branchNameInRemote Get a connection to that other branch in remote, and name it the same locally.

git fetch origin all branches (fetch so git status knows if you are behind or ahead on commits)

git fetch origin remotebranch just that branch from remote. Origin/master will have new changes, my master branch (local) will not.

git pull origin remotebranch pull from that to the branch where you are when you do it.

git pull short way to get the latest from the equivalent branch in remote, you are already on that branch locally.
```