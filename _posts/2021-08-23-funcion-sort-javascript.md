---
layout: post
title: Función sort() en Javascript
---

Si siempre tenés dudas como utilizar la función sort() en Javascript, no te viene mal leer el artículo.

La función sort() no crea una copia del array, sino que modifica el mismo array.
Entonces si queremos mantener el array original, tenemos que crear una copia.

```javascript
let arr = [9 , 3 , 1];
let sortedArr = [...arr].sort();

console.log(arr);
console.log(sortedArr);
```
Si llamamos al método sort() sin pasarle ningún parámetro, javascript va a comparar todos los elementos como si fuesen strings, primero las letras por orden alfabético, y luego los números. Si las letras están en mayúsculas, aparecen primero por importancia.

Volviendo al ejemplo de los números, si al método sort le pasamos una función de comparación, que recibe dos parámetros A y B que son las posiciones del array que recibirá para ir comparando mientras se ejecuta el sort. Esta función tiene que retornar cual de los dos elementos va primero que el otro.
Si retornamos un valor negativo significa que A va primero que B, si por el contrario retornamos un valor positivo, significa que B va primero que A y en el caso de ser iguales se retorna un 0

Si retomamos el ejemplo de los números, la función de comparación quedaría de la siguiente manera:

```javascript
let arr = [9 , 3 , 1];
let sortedArr = [...arr].sort((a, b) => {
  if(a < b) return -1;
  if(b < a) return 1;
  return 0;
});

console.log(arr);
console.log(sortedArr);
```
Podríamos mejorar la escritura de la función de comparación de números devolviendo la resta entre A y B.

```javascript
let arr = [9 , 3 , 1];
let sortedArr = [...arr].sort((a, b) => a - b);
```
Si un array tiene objetos tenemos que escribir una función de comparación que tenga en cuenta los atributos que nos interesan de ese objeto.

```javascript
let personas = [
  {nombre: “Juan”, edad:22},
  {nombre: “Pedro”, edad:16},
  {nombre: “Carlos”, edad:36},
];
personas.sort((a, b) => a.edad - b.edad);
```

Y si al ejemplo anterior queremos ordenarlo por el atributo nombre

```javascript
let personas = [
  {nombre: “Juan”, edad:22},
  {nombre: “Pedro”, edad:16},
  {nombre: “Carlos”, edad:36},
];
personas.sort((a, b) => {
  if(a.nombre < b.nombre) return -1;
  if(b.nombre < a.nombre) return 1;
  return 0;
});
```

Pero si tenemos en cuenta la forma de ordenar strings que tiene javascript, vamos a tener problemas con las letras mayúsculas, y palabras con tilde. Para esto tenemos que utilizar una función de los strings que se llama localeCompare() sirve para comparar strings sin tener en cuenta las mayúsculas, tildes ni otros símbolos especiales que puedan tener las letras.

```javascript
let personas = [
  {nombre: “Juan”, edad:22},
  {nombre: “Pedro”, edad:16},
  {nombre: “Carlos”, edad:36},
];
personas.sort((a, b) => a.localeCompare(b));
```

Y si tenemos elementos repetidos en nuestro array, y realmente nos importa respetar el orden original del mismo, lo primero que podemos hacer es verificar si otros atributos de nuestros objetos nos sirven para resolver la igualdad. En caso contrario podemos realizar un map() para agregarle un índice a cada elemento y cuando tengamos una igualdad podemos comparar por este atributo indice.

```javascript
let personas = [
  {nombre: “Carlos”, edad:36, pelo: “verde”},
  {nombre: “Pedro”, edad:16, pelo: “maron”},
  {nombre: “Carlos”, edad:36, pelo: “azul”},
];
personas.map((el, i) => {...el, key: i})
personas.sort((a, b) => {
  if(a.edad < b.edad) return -1;
  if(b.edad < a.edad) return -1;
  a.key - b.key;
});
```


