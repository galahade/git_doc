## How to setup git

### How to find git configuration variables

1. **/etc/gitconfig** file: Pass the option `--system` to `git config`, it reads and writes from this file.
2. **~/.gitconfig** or **~/.config/git/config** file: Pass the option `--global ` to `git config`, it reads and writes from this file.
3. **.git/config** file of the repository you’re currently using: Pass the option `--local ` to `git config`, it reads and writes from this file.

### How to setup git

The first thing you should do when you install Git is to set your user name and email address. This is important because every Git commit uses this information.

```
# To setup your name and email
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com

# To setup default text editor
git config --global core.editor emacs
```

### How to check git settings

```
# To list all git settings
git config --list

# To check specific key’s value
git config user.name

# To see which configuration file set specific key’s value
git config --show-origin rerere.autoUpdate
```

## How to manage files in local

### How to understand git data structure
1. **HEAD**: Last commit snapshot, next parent
2. **Index**: Proposed next commit snapshot
3. **Working Directory**: Sandbox
![git three tree](images/git_three_tree.png)

### How to create a local repository
```
# 1. Initialize the local directory as a Git repository.
git init

# 2. Add ignoring files 
$ cat .gitignore

```
You can find git ignore samples in [Ignore file reference](https://github.com/github/gitignore)

### How to stage files

```
# 1. Check files status to find out how many files should be staged.
git status 
# Or for short
git status -s

# 2. Check white space error
git diff --check

# 3. Stage files
# Stage files by name
git add <file>...

# Stage all unstaged files
git add .

# Stage part of file as a patch
git add -p

# or use this one to stage and commite files for shortcut
git commit -a

```

### How to View Changes

```
# To see what you’ve changed but not yet staged
git diff
# Or 
git diff <file>

# To see what you've staged but not yet commited
git diff --staged

# To see diff with external tools like emerge, vimdiff and many more.
git difftool

# To see which external diff tools is available on your system.
git difftool --tool-help
```


### How to commit files

```
# Commit the files that you've staged in your local repository.
git commit -m "First commit"
# Or 
git commit
# This will launches your editor set by shell’s EDITOR environment variable.
# usually vim or emacs
# You can configure it with
git config --global core.editor <editer_name>
```

### How to reverse changes

This is for redo the last commit: 

```
# 1. If change files
git add or git rm

# 2. If just run this, you can change commit message
git commit --amend
# Or use this to avoid change commit message
git commit --amend --no-edit
```

#### How to redo several commits

```
 git reset --soft HEAD~2 
```

```
# Discard changes in working Directory. This will replace working directory files with index.
git checkout -- <file>...

# Unstage files. This will replace index files with HEAD
git reset --mixed HEAD <file>...
# Or for shortcut
git reset <file>...

# Discard changes and unstage files. This will replace index and working directory files with HEAD. 
git reset --hard HEAD <file>...

# Undid the last commit. This will move HEAD to the parent of HEAD.
git reset --soft HEAD~

```

### How to remove files

```
# To remove files from index and working directory.
git rm <file>...
# Or force remove file 
git rm -f <file>...

# To remove files from index but keep in working directory. This is useful for forgeting ignore files.
git rm --cached <file>...

# To use Regular Expression in path.
git rm log/\*.log
git rm \*~
```

### How to move files

```
git mv <file_from> <file_to>
# It equels to 
mv README.md README
git rm README.md
git add README
```

### How to View commit history

```
# This is a best practice for now
git log --pretty=format:"%h - %an, %ar : %s" --graph 

# Check which branch are pointing which commit object
git log --oneline --decorate

# Show diverged history
git log --oneline --decorate --graph --all

# Show commits in one branch but not in master
git log --oneline --decorate --graph master..a_branch

# Show commits in A B but not in C
git log --oneline --decorate --graph refA refB ^refC
# equivalent
git log --oneline --decorate --graph refA refB --not refC

# Show commits in A but not in B and in B but not in A
git log --left-right --oneline --decorate --graph refA...refB
```
[Here](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2#rpretty_format) is useful options for `git log --pretty=format`  
[ Common options to `git log`](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History#log_options)  
[Options to limit the output of `git log`](https://git-scm.com/book/en/v2/ch00/limit_options)   

Here is a git log filter sample:

```
git log --pretty="%h - %s" \
   --author='Junio C Hamano' --since="2008-10-01" \
   --before="2008-11-01" --no-merges -- t/
```
double dashes (--) to separate the paths from the options.  
`--no-merges` is to prevent the display of  merged commits.

### How to Inspect commit
```
# To see HEAD move history
git reflog

# Then you can use command to see one commit
git show HEAD@{5}

# To see the previous commit
git show HEAD^
git show HEAD~

# with a number
# To see the second parent of HEAD when have more than one parent
git show HEAD^2
# To see “the first parent of the first parent,” or “the grandparent” 
git show HEAD~2
# OR
git show HEAD~~

# You can also combine these syntaxes like:
git show HEAD~3^2


# To see yesterday's commit
git show master@{yesterday}

# This command is sort of a Swiss army knife for inspecting Git objects
# -p instructs the command to first figure out the type of content, then display it appropriately
# -t instructs the object's type
git cat-file -p <SHA-1> or <HEAD>
# This command is show last commit's tree object
git cat-file -p master^{tree}
# it equals to
git ls-tree master
# Show tree objects recursively.
git ls-tree -r <SHA-1> or <HEAD>

# Show index's content
git ls-files -s
```

### How to rebase commit

```
# To rebase last 3 commits
git rebase -i HEAD~3
```

## How to use branch

### How to check branches

```
# List all branches
git branch

# List all branches with lats commit
git branch -v

# List branches are already merged into current branch
git branch --merged
# Or check with a branch name
git branch --merged master

# List branches haven't yet merged into current branch
git branch --no-merged
```

### How to create and delete a branch 

```
# Creat a branch in currnt commit
git branch <branch_name>

# Creat a branch and move HEAD to it
git checkout -b <branch_name>

# Move to an existing branch. This will move HEAD to point to <branch_name>
git checkout <branch_name>

# Remove a branch 
git branch -d <branch_name>
```

### How to merge a branch

```
# 1. checkout to the branch you want to use
git checkout master
# 2. merge a branch to it
git merge hotfix

# 3. If has conflicts, use
git mergetool
# Or edit conflict files and run git add
# To see mergetool config run
git mergetool --tool-help 
```

### How to rebase branch 

 There is no difference between __merge__ and __rebase__ in the end product of the integration, but rebasing makes for a cleaner history.

The principle of use rebase:  
    1.To rebase local change you've made but haven't shared yet before you push them.  
    2.Never rebase anything you've pushed somewhere.   
[Reference](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)   

```
# The steps of how to rebase a branch to master
$ git checkout {branch}
$ git rebase master
$ git checkout master
$ git merge {branch}

# More complacte example.
# This will rebase client to master which not on server.
git rebase --onto master server client

# Regase server to maseter
git rebase master server
git rebase <basebranch> <topicbranch>
```
[More complex example](https://git-scm.com/book/en/v2/Git-Branching-Rebasing#More-Interesting-Rebases)

## How to use remote repository

### How to Inspect remote repository

```
git remote
# or
git remote -v

git remote show <remote>

git ls-remote <remote>
```

### How to clone remote branch
```
# Create a remote named origin
$ git clone <remote-url>

# Create a remote named a spicified name
$ git clone -o <remote-name> <remote-url>
```

### How to add a remote repository

```
# git remote add <shortname> <url> 
git remote add origin `remote_repository_URL`

```

### How to fetch and pull from remote

```
# Fetch all new objects.
git fetch <remote>

# Fetch all remotes
git fetch --all

# Fetch and merge into current branch.
git pull <remote>
```

### How to push local commit to remote

```
# This may create a new branch in remote if there has no that banch.
git push <remote> <local_branch>
# Or use full form
git push <remote> <local_branch>:<remote_branch>

# Pushes the changes in your local repository up to the remote repository you specified as the origin
git push -u origin master

# If remote repository has contents before your first commit. Need pull first.
git pull --allow-unrelated-histories origin master

```

### How to rename and remove remote

```
# This changes all your remote-tracking branch names, too. 
git remote rename <old_name> <new_name>

# Remove a remote
git remote rm <remote_name>
```

### How to use remote branches
__Remote-tracking branches__: are local references that you can’t move, that references to the state of remote branches. They take the form of `<remote>/<branch>`

#### How to create a remote branch
```
# Use local branch to create a new remote branch
git push <remote> <local_branch>
# Or use full form, this can give remote branch a new name
git push <remote> <local_branch>:<remote_branch>
```

#### How to get a remote branch and track it in local

```
# 1. Get all new remote branch from remote
git fetch <remote>

# 2. Merge remote branch to current working branch
git merge <remote>/<remote_branch>
# Or for shorthand
git merge @{u}
# 2. Or Create a new remote-tracking branches
git checkout -b <tracking-branch> <remote>:<upstream_branch>
# for short use:
git checkout --track <remote>:<upstream_branch>
# Even shortcut for shortcut but need match two conditions:
# (a) doesn’t exist and (b) exactly matches a name on only one remote
git checkout <upstream_branch>
```

#### How to set a upstream branch for a already exited local branch

```
git branch -u origin/serverfix
```

#### How to see tracking branch

```
git branch -vv
# To get more recently result
git fetch --all; git branch -vv
```
## How to tag

### How to list tags

```
# List tags in alphabetical order
git tag
# Or  search for tags that match a particular pattern
git tag -l "v1.8.5*"
```

### How to create tags

There are two types of tags:

1. __lightweight tag__: a pointer to a specific commit.
2. __annotated tag__: have a full object and is generally recommended.

```
# Create a annotated tag in current commit
git tag -a v1.4 -m "tag message"

# Create a annotated tag in a commit
git tag -a v1.2 9fceb02

# Create a lightweight tag
git tag v1.4-lw

# Show tag infomation
git show v1.4
```

### How to share tags

```
git push origin v1.5

# push all tags at once
git push origin --tags
```

### How to delete tags

```
git tag -d v1.4-lw

# To update remote 
git push <remote> :refs/tags/<tagname>
git push origin :refs/tags/v1.4-lw
```

### How to checkout tags

```
# Creat a new branch and checkout tag
git checkout -b [branchname] [tagname]
git checkout -b version2 v2.0.0
```

## Will be removed

### Stash files

```
## Stash files
## Stash and clean indexed files
$ git stash push [message]
## Stash and clean files with untracked files --include-untracked or -u
$ git stash -u [message]
## Show what will be stash 
$ git stash --patch
## Don't stash anything that staged with git add command.(Rarely used)
$ git stash --keep-index
## Stash and clean files include ignored files
$ git stash --all

#Create a new branch from a stash and apply them, if successfully drop the stash
$ git stash branch {branch_name}

# Check stash record 
$ git stash list

# Apply the change you've stashed
## --index means keep staged changed after apply stash
$ git stash apply --index [stash name]
$ git stash pop --index [stash name] -- recommend this one. apply and drop 

# Remove a stash 
$ git stash drop [stash name]
```


## When you want to store changes in remote repository



### Delete untracked files and also any subdirectories that become empty as a result
```
#First use this command 
##Just show what would be done
$ git clean -d -n

#Then use this command which just replace -n with -f
## -f means force, -d means remove untracked directories in addition to untracked files
$ git clean -d -f 

#Or use this to do interactive clean
## -x means will remove files inclued ignored files.
$ git clean -d(-x) -i
```


## Commit code workflow.
### Before commit code

1. Use commnd `git diff --check` to check if your code has whitespace issues. If There some error like this: ![](https://git-scm.com/book/en/v2/book/05-distributed-git/images/git-diff-check.png)  Delete all whitespaces before you commit code.  
2. Don't mass multiple features to one commit. At lease one commit per issue with a useful message per commit. More complex option refer to: 
[Interactive Staging](https://git-scm.com/book/en/v2/Git-Tools-Interactive-Staging#_interactive_staging)

##Change history
### Undoing Things
```
# Commit new files with the same commit message
$ git commit --amend

# Unstaging a staged file
$ git reset HEAD {file_in_stage}
# Delete all files which haven't commit
$ git reset --hard HEAD

# Revert a unstaging file
$ git checkout -- {unstageing_file}

```

##Add Sub git repository to project
### sub-tree workflow
```
# list all remote repository
$ git remote -v

#Add new remote repository
$ git remote add --no-tags -f -t {remote_branch} {name} {remote_url}
git remote add --no-tags -f -t all_master ocs_us http://legitlab.letv.cn/ocs-us/ocs.git
git remote add --no-tags -f -t all_master ocs_api_us http://legitlab.letv.cn/ocs-us/ocsApi.git
git remote add --no-tags -f -t all_master ocs_worker_us http://legitlab.letv.cn/ocs-us/ocsworker.git
git remote add --no-tags -f -t all_master ocs_front_us http://legitlab.letv.cn/ocs-us/ocs-front.git

git remote add --no-tags -f -t all_master interfaceApp_us http://legitlab.letv.cn/ilemall_group/interfaceApp.git

#Create the subtree with git subtree add
git subtree add --prefix {dir_name} --squash {remote_name} [branch_name]

git subtree add --prefix ocs_zeus --squash ocs_us all_master
git subtree add --prefix ocs_api --squash ocs_api_us all_master
git subtree add --prefix ocs_worker --squash ocs_worker_us all_master
git subtree add --prefix ocs_front --squash ocs_front_us all_master
git subtree add --prefix interfaceApp --squash interfaceApp_us all_master

	```