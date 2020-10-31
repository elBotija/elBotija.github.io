---
layout: post
title: Duplicar objeto sin referencias en javascript 
---

Muchas veces necesitamos duplicar un objeto, pero el mismo nos gurda referencia al original. 

Acá vamos a dejar un truco simple, pero no por simple, menos efectivo. 


```javascript
const obj = {
  user: {
    name: 'jose',
    age: 20,
  },
  books: ["Don Quijote", "El cantar del mio cid"],
}

const cloneJson = (obj) => {
  return JSON.parse(JSON.stringify(obj));
}

console.log(cloneJson(obj));
```