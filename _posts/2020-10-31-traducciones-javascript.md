---
layout: post
title: Como crear una funcion multilenguaje en javascript
---

En algunas ocasiones necesitamos realizar una función para traducir los textos de nuestra app.

```javascript
const config = {lang: 'en'}

const translations = {
  es: {
    some_text: 'Algún texto',
    price_text: 'El precio es xxx'
  },
  en: {
    some_text: 'Some text'
    price_text: 'The price is xxx'
  },
};

const getTranslation = (key, patternText) => {
  let text = translations[config.lang][key] || translations['es'][key];
  if (patternText) {
    text = text.replace('xxx', patternText);
  }
  return text;
};

console.log(getTranslation(some_text));
console.log(getTranslation(price_text, '$11'));
```
