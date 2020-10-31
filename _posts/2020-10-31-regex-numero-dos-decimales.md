---
layout: post
title: regex String dejar numeros con dos decimales en javascript
---

En algunas ocaciones necesitamos utilizar una regex que devuelva solo números con dos decimales de un string.


```javascript  
const onlyTwoDecimals = (value) => {
  let str = value.replace(/[^0-9.]/g, '').replace(/(\..*)\./g, '$1');
  let n = str.indexOf('.') + 3;
  if (n === 2) {
    return str;
  }
  let res = str.substring(0, n);
  return res;
};
```
