---
layout: post
title: Arrancamos! Recorrer un objeto en javascript
---

Por que?, Porque siempre lo pensamos pero nunca lo hacemos.

Vamos a intentar recopilar pequeños aprendizajes del día a día.
En algunos casos, necesitamos recorrer todas los valores de un objeto. para ello podemos usar **Object.Keys**

```javascript  
  const obj = {
    name: 'juan', 
    age: 22,
  } 
  
  Object.keys(obj).map((key)=>{
    if(!obj[key].length){
      console.log(`El atributo es ${key} y su valor es ${obj[key]}`)
    }
  })
  ```
  
  En donde nos puede servir? Para validar un objeto que recibimos.
  ```javascript
  const validateObject = (obj) => {
    let err = []
    Object.keys(obj).map((e)=>{
      if(!obj[e] || !obj[e].length){
        err.push({text: `El campo ${e} es requerido`})
      }
    })
    return err
  }

  const product = {
    name: 'tomato',
    description: '',
    price: null
  } 

  validateObject(product);
  ```
