---
layout: post
title: Mi configuración de git
---

Shortcuts de git para configurar nuestro entorno de trabajo.

Tenemos que crear nuestro archivo .gitconfig en la carpeta home (linux).

```
[user]
name = <NuestroName>
email = <NuestroMail>

[color]
diff = auto
status = auto
branch = auto
ui = auto

[color "branch"]
current = yellow reverse
local = yellow
remote = green

[color "diff"]
meta = yellow bold
frag = magenta bold
old = red bold
new = green bold

[color "status"]
added = yellow
changed = green
untracked = cyan

[alias]
ci = commit
co = checkout
st = status
di = diff
lg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %Cblue%aN%Creset %s %Cgreen(%cr)%Creset' --abbrev-commit --date=relative

```
