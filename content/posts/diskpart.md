+++
title = 'Diskpart'
date = 2024-08-15T14:47:28+03:00
draft = false
+++
This page shows windows 11 disk apperance change things
## Change Disk Status Offline to Online
1. Win + R > open "run" > [write] `diskpart` and hit enter
![run Diskpart](/images/diskpart-0.png "run Diskpart")
1. [write] `list disk` 
![list disks](/images/diskpart-1.png "list disks")
1. Agree offline disk name, in my case is `disk 1` 
2. select the disk, [write] `select disk 1`
![select disk](/images/diskpart-2.png "select disk")
1. then change the status to online, [write] `online disk` and TA DA! Disk will be show in your computerr
![change online disk](/images/diskpart-3.png "change online disk") 