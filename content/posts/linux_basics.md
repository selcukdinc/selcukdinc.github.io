+++
title = 'Linux_basics'
date = 2025-03-26T17:43:03+03:00
draft = false
ShowToc = true
+++
#### Change Default Text Editor : `sudo update-alternatives --config editor`

#### Get current active proccess : `top` or `htop`

---

### _Folder Operations_

#### Command : '_mv_'
__mv__ have 2 abilitiy
- Rename file
  - `mv folder1 folder1_copied`
- Move file

mv how to get it move or rename ? ([source](https://unix.stackexchange.com/questions/61388/how-to-decide-that-mv-moves-into-a-directory-rather-than-replacing-directory/61402#61402)) 
`mv /test1 /test2`

- if `/test2` parameter is a **folder path** (_not a file_) _AND_ **exists** then always do 'move' (test2/test1)

- else if `/test2` parameter doesnt exists, then always do 'rename' ('test1' name to 'test2')

#### Copy Folder : `cp -r ./folder1 /home/user1`

#### Delete File | Directories : 
- single file : `rm /filepath/fileName`
- multiple Folder :`rm -r /filepath/folderName`

---

#### List Ports : `sudo lsof -i -P -n`

#### List Users : `cat /ect/passwd`

#### close ssh connection : `exit`

#### delete user : `userdel username`

--- 

### Case : Create Sudo user
"Warning! current user must be have sudo privileges"
- Create user : `sudo adduser user1`
  - you can skip up to password area
- Add user Sudoers : `sudo usermod -aG sudo user1`
- Verify sudo access : `su - user1`
  - then : `sudo ls /root`

---

## For PM2
#### Access gui : `pm2 monit`
#### start proccess : `pm2 start something`