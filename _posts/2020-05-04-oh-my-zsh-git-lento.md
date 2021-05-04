---
layout: post
title: Oh My Zsh acciones sen git lentas
---

Si usas Oh My Zsh y tienes problemas al actualizar repositorios git.
Seguramente tienes problemas con:
    git_prompt_info()
    parse_git_dirty()

Prueba deshabilitandolas globalmente con el siguiente comando.

```bash
git config --add oh-my-zsh.hide-status 1
git config --add oh-my-zsh.hide-dirty 1
```
