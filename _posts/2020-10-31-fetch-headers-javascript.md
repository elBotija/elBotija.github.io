---
layout: post
title: Crear una funcion para estandarizar los headers de las request en javascript
---

**No** repetir codigo!

Para no escribir reiteradas veces lo mismo, podemos crear una funcion que nos devuelva los headers que necesitan nuestros request.
Si la api tiene un token, al obtenerlo, podemos guardarlo y agregarlo en esta misma función.

```javascript
const fetchHeaders = () => {
  let headers = {
    Accept: 'application/json',
    'Content-Type': 'application/json',
  };
  return headers;
};
```
