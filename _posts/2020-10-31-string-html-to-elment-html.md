---
layout: post
title: Convertir un texto html a un elemento html con Javascript 
---

En algunas ocasiones necesitamos convertir un string que contiene html a un elemento html. 


[**DOMParser**](https://developer.mozilla.org/es/docs/Web/API/DOMParser) puede analizar gramaticalmente (parsear, en adelante) código XML o HTML almacenado en una cadena de texto y convertirlo en un Documento DOM. DOMParser está especificado en DOM Parsing and Serialization.


```javascript
const stringToHTML = function (str) {
  const parser = new DOMParser();
  const doc = parser.parseFromString(str, 'text/html');
  return doc.body;
};
```

Un ejemplo para utilizar la función sería el siguiente. 

```javascript
const textLink = '<a href="https://elbotija.github.io/" target="_blanck">elBotija blog</a>';
let link = stringToHTML(textLink);
link = link.querySelector('a');

```

En el siguiente [**link**](https://jsfiddle.net/59c7hfed/), dejamos el ejemplo funcional. 