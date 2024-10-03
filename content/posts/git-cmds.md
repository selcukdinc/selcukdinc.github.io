+++
title = 'Git Cmd Notes'
date = 2024-08-08T00:05:40+03:00
draft = false
+++

### Basics
- [Create git repo] > `git init`

- [see what happened your git] > `git status`

- [track or update files]
```
[for all unstaged files]

git add .

[for specified files]

git add .\example.txt

```

- [.gitignore file] (dont add  unwanted files to repo, create .gitignore file) >
```
[inside of .gitignore file]
folder/unwantedNote.txt
itsWholeFolder/
```

```
# ignore all .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in any directory named build
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory and any of its subdirectories
doc/**/*.pdf
```
[source of above infos](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository) 
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