# git-cheatsheet
A cheatsheet for git

#Config
* ```git config --list``` List the current configs
* ```git config --global color.ui true``` set this to see colours in git shell messages
* ```git config --global user.name "Yang Li"``` Sets up name of the git user
* ```git config --global user.email "yangli1990@live.com.au"``` Sets up the email of the git user
* ```git config --global core.editor subl``` Configure your git editor (You will need this for interactive rebase)
* ```git config --global merge.tool opendiff``` Configure your merge tool
* ```git config --global core.autocrlf input``` activate this on linux/osx to change any windows CRLF into LF
* ```git config --global core.autocrlf true``` use this on windows to change CRLF to LF when committing
* ```git config user.email "bob@live.com.au"``` Will override your global config settings, check by using ```git config user.email```
* ```git config --global alias.mylog "log --oneline"``` Creates an alias command that can be called by ```git mylog```

#Log
Log will show you a chronological order of all of your commits.  Hit the 'q' key to get out of log
```
git log
```

Here are some ways to customize your git log messages
* ```git log --oneline``` Shorten the log messages so it becomes one line per commit
* ```git log --pretty=format: "%h %ad- %s [%an]"``` Form your output the way you want it by using these placeholders
  * %ad - date
  * %an - name of author
  * %h - SHA Hash, or commit id
  * %s - message
  * %r - ref names
* ```git log --oneline -p``` Will show you one line and the changes in that commit
* ```git log --oneline --stat``` Will show how many incertions/deletions were made per line
* ```git log --oneline --graph``` Will show you a visualisation of the branches and commits
* ```git log --until=1.minute.ago``` Shows commits before 1 minute ago
* ```git log --since=1.day.ago``` Shows commits starting from a day ago
* ```git log --since=2001-12-20 --until=5.weeks.ago``` Shows commits between 2001-12-20 to 5 weeks ago

#diff
```git diff``` will show you the last commit
```git diff test/index.html``` specify the file
```git diff HEAD~3``` The 3rd diffs
```git diff HEAD~3..HEAD~2``` Compare the 3rd most recent commit to the 2nd most recent commit
```git diff <sha1>..<sha2>``` Compare commits based on their commit id
```git diff master branch-dev``` Compare master branch with the branch-dev branch

#blame
```git blame index.html``` gets a list of who made what changes

#.gitignore
* You can add a list of files to be excluded in a .gitignore file
```
node_modules/ //ignores all node_modules files
*.log //ignores all log files
```
* You can also ignore files personally by adding them to the .git/info/exclude file

#Rebase
Rebase allows you to combine your commits together

```
git rebase <branch>
```

##Interactive Rebase

```
git rebase -i HEAD<num>
```
Examples
```
git rebase -i HEAD~3
git rebase -i HEAD^^^
```

###Interactive Choices
* pick - picks that change
* edit - stops at this change
* reword - lets you change the commit message
* squash - combines this change with its parent

#Stash
Stash allows you to store changed files in a temporary area.  This can be used to transfer files from branches.

* ```git stash save``` Git stash saves all of your files on the stack
  * ```[--include-untracked]``` saves untracked changes too
  * ```[--keep-index]``` saves only unstaged changes
* ```git stash list``` Will list all of your stashs 
  * ```[--stat]```
* ```git stash apply``` Will unstash and retrieve your last stashed files
* ```git stash apply stash@{<num>}``` Allows you to choose which stash to apply
* ```git stash drop``` Allows you to drop the last stashed item
* ```git stash drop stash@{<num>}``` Allows you to pick an stashed item to drop
* ```git stash pop``` Combines apply and drop
* ```git stash show``` Shows details of the most recent stash
* ```git stash show stash@{<num>}``` Shows the stash chosen
* ```git stash branch <name> stash@{<num>}``` creates a new branch and unstashs
* ```git stash clear``` kills all your stash

#Cherry Pick
cherry picking lets you pick out specific commits from various branches to use.

```git cherry-pick <hash>```

e.g

```git cherry-pick 3532e12```

###Changing your commit message

```git cherry-pick --edit <hash>```

###Cherry picking multiple commits

```git cherry-pick --no-commit <hash> <hash> ...```

###Cherry pick author
Because Cherry Picking retains the original author, you might want to add the Cherry picker as an additional author by using the -x option

```git cherry-pick -signoff <hash>```

#Removing history
```git filter-branch --tree-filter <command>```

e.g

1. ```git filter-branch --tree-filter 'rm -f password.txt' -- --all``` Will go through and remove and password.txt files in your commits, (-- --all will run this in all commits on all of our branches, we could change --all to HEAD) 
2. ```git filter-branch --index-filter 'git rm --cached --ignore-unmatch password.txt'``` Same effect as above
3. ```git filter-branch --tree-filter 'find . -name ".*mp4" exec rm {}\;'``` Remove all files related to mp4
4. ```git filter-branch --prune-empty -- --all``` Removes any commits that might be an empty commit

#Reflog  (pronounced ref-log)

Reflog allows you to unreset commits, in essence it's a secret repository of the commits you thought you deleted.

```git reflog``` this command will list all of your commits and resets in order

To get back your changes use ```git reset --hard <sha>``` instead of the ```<sha>``` you could use ```HEAD@{<num>}```

```git log --walk-reflogs``` For a more verbose version of reflog
