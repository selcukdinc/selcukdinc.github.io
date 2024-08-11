+++
title = 'Hugo Basics'
date = 2024-08-02T08:00:06+03:00
draft = false
+++
[edit2 (04.08.2024)] ~
### Install Hugo on Macos 
Easy installation with brew, if your pc doesnt recognise brew, firstly install package manager brew [here](https://brew.sh)
```
brew install hugo
```
### Install Hugo on Windows
Easy installation with chocolatey, if your pc doesnt recgnoise chocolatey, firstly install package manager cohoco in [here](https://chocolatey.org/install). tip : use admin powershell
```
choco install hugo-extended
```

[hugo installation page](https://gohugo.io/categories/installation/)

~
## First Thing First
Actually first of all you know how do start localhost your web site. And when your start your host, your files can be hot reload during server live. Easy to use. Write terminal 

[edit2 (04.08.2024)] (terminal opened project location)
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
[edit1 (03.08.2024)]
upss if you dont re-deploy on github actions, site will be expired and new post doesnt reloaded. in my case, resolve path is
```
github > actions > Deploy Hugo site to Pages > Run workflow
```
if your case 'Deploy Hugo site to Pages' doesnt appear firstly add this workflow in github, here is the [guide](https://gohugo.io/hosting-and-deployment/hosting-on-github/)

## Goldmark Extensions
### Definition Lists (default Enabled)
Apple
:   Example of definition list with apple

Orange
:   Expamle of definition list with orange
```
Apple
:   Example of definition list with apple

Orange
:   Expamle of definition list with orange
```

### Foot Notes (default Enabled)

That's some text with a footnote.[^1]

[^1]: And that's the footnote.

```
That's some text with a footnote.[^1]

[^1]: And that's the footnote.
```
### Tables (default Enabled)
X    | Column1  | Column2
--   | --       | -- 
Row1 | foo      | bar
Row2 | baz      | bim
```
X    | Column1  | Column2
--   | --       | -- 
Row1 | foo      | bar
Row2 | baz      | bim
```
### Task Lists (default Enabled)
- [ ] First step pragramming
  - [x] print 'Hello World'
  - [ ] Learn variable types
- [ ] Create cmd base rpg game 
```
- [ ] First step pragramming
  - [x] print 'Hello World'
  - [ ] Learn variable types
- [ ] Create cmd base rpg game 
```
