---
layout: post
title: GIT - Listar archivos modificados entre commits
---

Algunas veces necesitamos listar los archivos que fueron modificados entre dos commits

En una terminal parados en el repositorio git escribimos lo siguiente

```bash  
  git diff --name-only SHA1 SHA2
  ```
  
  SHA es la key/hash del commit, si queremos comparar contra dev simplemente solo escribimos la SHA del commit a comparar.
  
  ```bash
  git diff --name-only SHA1
  ```
