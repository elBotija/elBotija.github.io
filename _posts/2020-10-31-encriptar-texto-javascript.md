---
layout: post
title: Arrancamos! Recorrer un objeto en javascript
---

En algunas ocasiones necesitamos encriptar algun texto, de forma simple en el front

```javascript
const encrypted = (string, encode = true) => {
  if (!string) return null;
  let text = string;
  let alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz'.split('');
  let rot13Alphabet = 'LMPQRSTUVWXYZABCDEFGHIJKNOnopqrstuvwxyzabcdefghijklm'.split('');
  if (encode) {
    rot13Alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz'.split('');
    alphabet = 'LMPQRSTUVWXYZABCDEFGHIJKNOnopqrstuvwxyzabcdefghijklm'.split('');
    text = btoa(text);
  }
  let result = '';
  for (let x = 0; x < text.length; x++) {
    let match = false;
    for (let y = 0; y < alphabet.length; y++) {
      if (text[x] === alphabet[y]) {
        result += rot13Alphabet[y];
        match = true;
      }
    }
    if (!match) {
      result += text[x];
    }
  }
  if (!encode) {
    result = atob(result);
  }
  return result;
};

export { encrypted };

```
