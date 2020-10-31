---
layout: post
title: Función para validar CUIT con javascript
---

**CUIT** son las siglas de Clave Única de Identificación Tributaria y se trata de un número usado en el sistema tributario argentino para identificar a las personas físicas o jurídicas autónomas.

```javascript
const verifyCuit = (cuit) => {
  if (cuit.length !== 11) {
    return false;
  }

  let acumulado = 0;
  let digitos = cuit.split('');
  let digito = parseInt(digitos.pop());

  for (let i = 0; i < digitos.length; i++) {
    acumulado += digitos[9 - i] * (2 + (i % 6));
  }

  let verif = 11 - (acumulado % 11);
  if (verif === 11) {
    verif = 0;
  } else if (verif === 10) {
    verif = 9;
  }

  return digito === verif;
};

export { verifyCuit };
```
