---
layout: post
title: Configurar nombre/mail en repositorio GIT
---

En algunas ocasiones necesitamos configurar distintos username/mail en los repos de git.

Los cambios los realizamos desde la terminal.

De manera global
```bash
git config --global user.name "Nombre"
git config --global user.email "mail@example.com"
```

Solo en el repositorio donde estamos parados
```bash
git config user.name "Nombre"
git config user.email "mail@example.com"
```

Verificamos los cambios
```bash
cat .git/config
```


