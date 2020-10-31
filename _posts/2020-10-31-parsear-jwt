---
layout: post
title: Parsear JWT en javascript
---

En algunas ocaciones necesitamos parsear/decodificar un **JWT** con javascript

```javascript
const parseJWT = (token, tokenName) => {
  let base64Url = token.split(tokenName);
  base64Url = base64Url[1].split('.')[1];
  var base64 = base64Url.replace(/-/g, '+').replace(/_/g, '/');
  var jsonPayload = decodeURIComponent(
    atob(base64)
      .split('')
      .map(function (c) {
        return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
      })
      .join('')
  );
  return JSON.parse(jsonPayload);
};
```
