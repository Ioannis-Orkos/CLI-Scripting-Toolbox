# GIT/GIT-Hub


 Git is version control system

There are different types
- Centralized - single repository , dependent on central server
    - Subversion
    - Team foundation system
- Distributed - Uses a centralized workflow the benefit is no single point failure
    - Git
    - Mercurial


### How integration manager workflow works

Contributors(have pull access) - fork the project to their a git repo and clone it to their local repo. <br>
After making changes they push their local repo to the forked repo and send pull request to the maintainers of the project. <br>
Maintainers(have a push access) get notified and pull the contributors project to their local repo. <br>
After review they merge the contributors repo to their local repo and push the merged project to sent to centralized repo. <br>
#

## Git tools

- Git command line -> Git Bash is a better tool than command line , the commands are like terminal.
- Power shell with posh-git module to have better representation.
- VS-code with git-lens extension
- Git-Credential-Manager-for-Windows To save git credentials
- GUI tools
    - Git-kraken is free for open source
    - Source-tree
    - Merge conflict Tools
    - P4Merge
    - Kdiff
    - WinMerge(windows olny)
#




### Configuration

```cmd
git --version
```



### Settings

- --system all users
-  --global all repositories of the current user
- --local current repositories

```cmd

git config --global user.name "Ioannis Orkos"
git config --global user.email yohannisnicos@gmail.com

    # for vim editor
    git config --global core.editor "vim"
    git config --global core.editor "vim --wait"

    # for visual code editor
    git config --global core.editor "code"
    git config --global core.editor "code --wait"
        # wait to exit and set vs-code as a primary editor


git config --global -e
    # edit global settings on editor
git config --list


windows return line with \n \r
mac return line \n
-end of lines /r carriage return(CR)
              /n line feed(LF)

    # to fix the return line issue
    git config --global core.autocrlf true # for windows
    git config --global core.aurocrlf input # for mac


# Need correction after in config file
# to show diff on the editor using visual code
git config --global diff.tool vscode
git config --global diff.tool.vscode.cmd "code --wait --diff $LOCAL $REMOTE"


# Need correction after in config file
# Merge tool
git config --global merge.tool p4merge
git config --global merge.tool.p4merge.path "C:\Program Files\Perforce\p4merge.exe"
git config --global mergetool.keepBackup false


#Creating an alias
git config --global alias.lg “log --oneline"


# To config fast forward merge
    config ff no
        # for the current repository
    config --global ff no
        # for the user repository


git config --help # brif help
git config -h # quick help

```



###  Git config file content

```cmd

# This is Git's per-user configuration file.

[user]
    name = Ioannis Orkos
    email = yohannisnicos@gmail.com

# Please adapt and uncomment the following lines:
# name = unknown
# email = Ioannis@LAPTOP-DILM6GQH.(none)

[core]
    editor = code --wait
    autocrlf = true

[diff]
    tool = vscode

[difftool "vscode"]
    cmd = "code --wait --diff $LOCAL $REMOTE"

[merge]
    tool = p4merge

[mergetool "p4merge"]
    path = "C:\\Program Files\\Perforce\\p4merge.exe"

```



### Terminal basic command help

```cmd

pwd # present working directory

echo #to display and also to write on a file
    echo help > h.txt #write new
    echo help2 >> h.txt #append

cd #change directory
    - cd .. (with two dots) to move one directory up
    - cd to go straight to the home folder
    - cd- (with a hyphen) to move to your previous directory

ls #list file - directory
    - ls -R will list all the files in the sub-directories as well
    - ls -a will show the hidden files
    - ls -al will list the files and directories with detailed information like the
permissions, size, owner, etc.

mkdir #make directory

rm #remove or to del file
    -rm -r #to remove file and folders
    -rm -rf #to remove file and folders and force the process

mv #

```


### How Git works


Project directory--(add)-->Staging area/Index--(commit)-->GIT Repository

- commit is like taking a snap shot of a project
- staging area allows us to review our code before commit
- commit often with meaningful message
- each commit should represent one change
- use present or past statement eg. fix the bug or bug fixed

> Master = main branch <br>
> Head reference to the current branch

### Git - Initialize, Add, Commit
```cmd

git init

-- is like a separation when git confused space both sides

echo hello > file1.txt
echo hello > file2.txt

# to add to the stage area
git add .
    # add all
git add file1.txt file2.txt
git add *.txt
    # all with txt extention

# to permanently store to the repository area
git commit -m "Initial commit."
git commit
    # this opens our default editor and we can add detaileds comment in the file
git commit -am "Initial commit" or git commit -a -m "initial commit"
    # scaping the stage area
```


### Git - Move, Del, Rename
```cmd
# one we delete a file we need to add to the staging area and commit the deleted file to notify the change made
# the other option is to use

git rm file1.txt
    # remove from both working and staging dirctory
git rm --cached file1.txt
    # remove from staging

# if we move file or rename file we need to add both the privious name and new name or location
# the other option is to use

git mv file1.txt file1.txt

# to get help
git rm -h

# to remove folder form the staging area

```


### Git - Status, List staged and committed files
```cmd

git status
    # to check the of the staging files

git status -s
    # short A-add M-modifed ??-untracked

git ls-files
    # list files at the staging area

git ls-tree HEAD #list all the files in the commit point
    #files reprented using blobs and folders using trees
    # -r to include all the sub dir

```


### .gitignore # only works when files are not in the stage
```cmd

logs/

main.log

*.log

```


### Git - Diff, Diff tool
```cmd

# to compare staged to commited(repository)
git diff --staged
git diff --staged file.txt

# to compare working dir to staged
git diff
git diff file.txt

# to compare between two commits
git diff HEAD~2 HEAD
git diff HEAD~2 HEAD file.txt

#to see diff on editor
git difftool
git difftool --staged

```
### Git - Log ~repository
```cmd
# To see the commit history

git log
git log --oneline
git log --oneline --reverse
git log --oneline --all
    #to see all logs including other branches
git log --oneline --all --graph
    #to see all logs including other branches also add rep of brach path

git log --stat
    # Shows the list of modified files
git log --patch
    # Shows the actual changes (patches)

```

```cmd
# Filtering the history

git log -3
    # Shows the last 3 entries
git log --author=“Mosh”
git log --before=“2020-08-17”
git log --after=“one week ago”
git log --grep=“GUI”
    # Commits with “GUI” in their message
git log -S“search”
    # Commits with “GUI” in their patches
git log hash1..hash2
    # Range of commits

#Formatting the log output
    git log --pretty=format:”%an committed %H”

```


```cmd

# Viewing the history of a file

git log file.txt
git log --oneline -- file.txt
    # Shows the commits that touched file.txt
git log --stat file.txt
    # Shows statistics (the number of changes) for file.txt
git log --patch file.txt
    # Shows the patches (changes) applied to file.txt
git log --oneline --all --graph --name-status
    # List commit hestory with file names and thier status

```

### Git - Show ~repository
```cmd
git show f850175 #log id
git show HEAD
git show HEAD~1 #one step back form log
git show HEAD~1:file1.txt #tosee the exact file on that point
git show f850175:file1.txt

git ls-tree HEAD #list all the files in the commit point
                 #files reprented using blobs and folders using trees
                 # -r to include all the sub dir

```

### Git - Restore, Clean
```cmd
# to restore files form other branches or commits we use checkout command

git restore --stage file1.txt
    # undo from commit history to the staging area

git restore file1.txt
    # undo the file from staging area

git restore --source=HEAD~1 file4.txt
    # undo the file change to the privious snapshot
    # the file will be added to the working dir

git restore --source=branchname -- file
    # can restore file from other branch using branchname andcommit history

git clean -fd
    # remove all untracked files

git checkout f3ffa45 file
    # restore with checkout from another branch or commit history
    # the file will be added to both staging and working dir

```

### Git - Checkout
```cmd

git checkout dad47ed
    # Checks out the given commit becareful not to make a commit change
    # Just to look around

git checkout master
    # Checks out the master branch/reset back where it was
    #restore with checkout from anouter branch or commit history

git checkout f3ffa45 file
    #file need to commit after

```


### Git - contributors
```cmd

git shortlog
git blame file.txt
    # Shows the author of each line in file.txt

```S

### Git - Tag
```cmd
git tag v1.0
git tag -a v1.0 -m "message"
# Tags the last commit as v1.0
git tag v1.0 5e7a828
# Tags an earlier commit
git tag
git tag -n
# Lists all the tags -n includes the meassage
git show v1.1
#the show the detail info about the tag including commit point
git tag -d v1.0
# Deletes the given tag
```


### Git - Finding a bad commit

```cmd
git bisect start
git bisect bad
# Marks the current commit as a bad commit
git bisect good ca49180
# Marks the given commit as a good commit
git bisect reset
# Terminates the bisect session
```

### Git - Branch creation
```cmd
git branch branch-1
# Creates a new branch called bugfix
git branch -m branch-1 branch-2
# To rename branch
git checkout branch-1
# Switches to the bugfix branch
git switch branch-1
# Same as the checkout
git switch -C branch-1
# Creates and switches
git branch -d bugfix
# Deletes the bugfix branch
# Capital D to Force del branch
git branch
# list avalable branches
```

### Git - Compare branches
```cmd
git log master..branch-1
# show all the commites that are in branch-1 but not in master
git diff master..branch-1
git difftool master..branch-1
git diff --name-status master..branch-1
```

### Git - Stashing

> useful when switching branches

```git
git stash push -m “New tax rules”
# Creates a new stash
git stash list
# Lists all the stashes
git stash show stash@{1}
# Shows the given stash
git stash show 1
# shortcut for stash@{1}
git stash apply 1
# Applies the given stash to the working dir
git stash drop 1
# Deletes the given stash
git stash clear
# Deletes all the stashes
```



### Git merging
- Fast forward merging
    - if branches have not diverged and direct path
    - moves the pointer forward
- Three way merging
    - if branches diverged
    - git creates new commit
    - based on previous snapshot git creates a new commit called merge commit



### Git - Merging
```git
git merge bugfix
# Merges the bugfix branch into the current branch
git merge --no-ff bugfix
# Creates a merge commit even if FF is possible
# To config fast forward merge
config ff no
# for the current repository
config --global ff no
# for the user repository
# Git merge conflict managment merge tool
# Git config before using merge tool
git mergetool
Fast forward merging
if branches have not diverged and direct path
moves the pointer forward
Three way merging
if branches diverged
git creates new commit
based on previous snapshot git creates a new commit called merge commit
git merge --abort
# Abort merging process
git merge --squash bugfix
# Performs a squash merge
# This squash braches and add it to the base branch with our merging
# Most of the time requires deletion
```


### Git - View merged branches
```git
git branch --merged
# Shows the merged branches
git branch --no-merged
# Shows the unmerged branches
```


### Git - Undo Changes ( Reset , Revert , Cherry pick ,Reset )
```git
git reset --hard head~1
# Reset soft - changes the snapshot(repository)
mixed - default stage + repository
hard - all work dir, stage and repository
# Take back to the privious commit changes history
git revert -m 1 Head
# back one from head
# Revert won't change the history
# chages the commit to go back and commit ahead
git rebase master
git mergetool
git rebase --continue
git rebase --abort
# moves the current branch to the given branch
# use mergetool to resolve conflict
# revert continure till all resolved
# skip if needed to stop with out reveting back to the orgnal postion
git cherry-pick 567ec
git mergetool
# get he files form other commit to the current commit
# commit after resolving conflictes
# can also be aborted
git restore --source=branchname -- file
# to restore file from other branch
```



## GIT-Hub

- Create a repo
- Add collaborates

```git
git clone https://github.com/YohannisNicos-IoannisOrkos/mars.git
# to copy git project to local machine
git remote
# Shows remote tacking repos
git remote -v
# Shows remote tacking url
git fetch origin master
# updates local master branchbut not merge it
# Fetches master from origin
git fetch origin
git fetch
# Fetches all objects from origin
git pull origin
# equivalent to fetch and merge
git pull --rebase
# retches and move the current banch on top of the comming branch
git push origin main
# push local changes to remote origin main
# needs credintal
git push
# Shortcut for “git push origin master”
git push -f
#this forces push
#never do this if there is a conflict pull firsr and resolve the conflict
git config --global creadential.helper cache
for permant use git credential manager for windows
git tag v1.0
# set tag on local repo
git push origin v1.0
# to push local tag to the remote repo
git tag -d v1.0
Create a repo
Add collaborates
# to delete tag from local repo
git push origin --delete v1.0
# to delete tag from remote repo
release found on git-hub which based on tags
git branch -vv
# Shows local & remote branches
# this helps to show remote and local tanglement
git branch
# shows local branches
git branch -r
# Shows remote branches
git push -u origin earth
# Pushes earth branch to origin
git push -d origin earth
# removes earth branch from origin
git switch -c earth origin/earth
# after fetch we get remote track for new branch(if added) from remote
this command creates new branch on local repo to track the origin repo
git branch --set-upstream-to=origin/main
# incase we loss link of the remote tracking to main this will bind it back
git remote add upstream url
# Adds a new remote called upstream
git remote rm upstream
# Remotes upstream
git remote prune origin
# this will remove no longer used remote url
# useful to remove remote tracking after deleting branch from origin
```

### Collaboration