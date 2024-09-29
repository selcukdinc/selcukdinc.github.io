+++
title = 'Git Cmd Notes'
date = 2024-08-08T00:05:40+03:00
draft = false
+++

### Basics
[Create git repo] > `git init`

[.gitignore file] (dont add or remove files from repo, create .gitignore file) >
```
folder/unwantedNote.txt
itsWholeFolder/
```
if your add and commited before unwanted files, should be remove cached files
### Basic create new branch and switch to the new branch

[Create and Switch branch] > `git checkout -b new_branch`

this command shortcut of these

[Create Branch] > `git branch new_branch`

[Switch Branch] > `git checkout new_branch`

[soruce : https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)

### Case : "new_branch" minor changes add to the main branch
* make sure right branch, `git status` 
* make new changes "new_branch"
* add changes to the "new_branch" > `git add .`
* commit changes > `git commit -m "new_branch : x changed to y"`
  
Change the target branch, this case is 'main' branch

[switch branch] > `git checkout main`

[merge branch] > `git merge new_branch`

changes added main branch congrats!

### Case : Remove Cached Files 
* for file
```
git rm --cached singlefile.txt
```
* for folder
```
git rm --cached -r .\hunderedTinyFiles\
```