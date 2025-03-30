+++
title = 'Linux_basics'
date = 2025-03-26T17:43:03+03:00
draft = false
ShowToc = true
+++
#### Change Default Text Editor : `sudo update-alternatives --config editor`

#### Get current active proccess : `top` or `htop`

#### Copy Folder : `cp -r ./folder1 /home/user1`

#### Rename Folder : `mv folder1 folder1_copied`

#### List Ports : `sudo lsof -i -P -n`

#### List Users : `cat /ect/passwd`

#### close ssh connection : `exit`

#### delete user : `userdel username`

### Case : Create Sudo user
"Warning! current user must be have sudo privileges"
- Create user : `sudo adduser user1`
  - you can skip up to password area
- Add user Sudoers : `sudo usermod -aG sudo user1`
- Verify sudo access : `su - user1`
  - then : `sudo ls /root`
  
## For PM2
#### Access gui : `pm2 monit`
#### start proccess : `pm2 start something`