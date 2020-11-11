---
layout: post
title: Popular pseudoelementos mediante attr con CSS
---

Muchas veces necesitamos agregar contenido mediante pseudoelementos de manera dinamica con css.

```html
<span data-dinamic="User: ">elBotija</span>
```
```css
span:before{
  content: attr(data-dinamic);
}
```