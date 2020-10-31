---
layout: post
title: regex String dejar solo numeros en javascript
---

En algunas ocaciones necesitamos utilizar una regex que devuelva solo números de un string.


```javascript  
const onlyNumbers = (value) => {
  return value.replace(/[^\d.-.]/gi, '').replace(/(\..*)\./g, '$1');
};
```
