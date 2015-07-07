# git-cheatsheet
A cheatsheet for git

#Config
* ```git config --global core.autocrlf input``` activate this on linux/osx to change any windows CRLF into LF
* ```git config --global core.autocrlf true``` use this on windows to change CRLF to LF when committing

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


#Removing history
```git filter-branch --tree-filter <command>```

e.g

1. ```git filter-branch --tree-filter 'rm -f password.txt' -- --all``` Will go through and remove and password.txt files in your commits, (-- --all will run this in all commits on all of our branches, we could change --all to HEAD) 
2. ```git filter-branch --index-filter 'git rm --cached --ignore-unmatch password.txt'``` Same effect as above
3. ```git filter-branch --tree-filter 'find . -name ".*mp4" exec rm {}\;'``` Remove all files related to mp4
4. ```git filter-branch --prune-empty -- --all``` Removes any commits that might be an empty commit
