+++
title = 'Hugo Basics'
date = 2024-08-02T08:00:06+03:00
draft = false
+++
## First Thing First
Actually first of all you know how do start localhost your web site. And when your start your host, your files can be hot reload during server live. Easy to use. Write
```
hugo server
```

## Create New Post
When we need post new something, open your terminal in your hugo project folder and write the command below.
```
hugo new posts/new-file.md
```
and hugo automatically create file like this
```
+++
title = 'Hugo Basics'
date = 2024-07-30T21:35:06+03:00
draft = false
+++
```
Yeah i discover all ready exist feature

if your 'date = 2024-07-30T21:35:06+03:00' parameter set the future and draft is be false post doesnt show in the web, when date time pass the setted date, new post will be show in your blog!
>and i set the morning post time, see you soon reader!

in my case i set the date like this
```
date = 2024-08-02T08:00:06+03:00
```