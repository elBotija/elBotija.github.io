---
layout: post
title: Mostrar el brach en la terminal en un repositorio git
---

Es muy útil mostrar el branch en el cual nos encontramos trabajando en la terminal. 

En el archivo .bashrc || .bash_profile

```
function parse_git_branch_and_add_brackets {
  git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\ \[\1\]/'
}

##PS1="\h:\W \u\[\033[0;32m\]\$(parse_git_branch_and_add_brackets) \[\033[0m\]\$ "
PS1="\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\[\033[0;32m\]\$(parse_git_branch_and_add_brackets) \[\033[0m\]\$ "
```

Pongo las dos, por si una no les funciona, dependiendo el sistema.
```
function parse_git_branch_and_add_brackets {
  git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\ \[\1\]/'
}

##PS1="\h:\W \u\[\033[0;32m\]\$(parse_git_branch_and_add_brackets) \[\033[0m\]\$ "
PS1="${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\[\033[0;32m\]\$(parse_git_branch_and_add_brackets) \[\033[0m\]\$ "
```
