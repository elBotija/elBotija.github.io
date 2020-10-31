---
layout: post
title: Función para validar un mail con javascript
---

En algunas ocasiones necesitamos validar un mail antes de ser enviado/guardado.


El método [**HTMLSelectElement.checkValidity()**](https://developer.mozilla.org/es/docs/Web/API/HTMLSelectElement/checkValidity) comprueba si el elemento tiene restricciones y si las cumple. Si el elemento no cumple sus restricciones, el navegador lanza un evento cancelable invalid al momento y luego devuelve false.

```javascript
const verifyMail = (mail) => {
  if (mail.length < 50 && mail.indexOf('.') !== -1) {
    let elem = document.createElement('input');
    elem.type = 'email';
    elem.value = mail;
    elem.required = '';
    elem.style = 'display:none';
    return elem.checkValidity();
  }
  return false;
};

```
