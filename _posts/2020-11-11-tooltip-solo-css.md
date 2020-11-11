---
layout: post
title: Crear tooltip solo con CSS
---

Algunas veces necesitamos agregar un tooltip, y podemos hacerlo solo con css.

```html
<span data-dinamic="Tooltip">elBotija</span>
```
```css
span {
  position: relative;
}
span:before {
  content: attr(data-dinamic);
  position: absolute;
  width: max-content;
  padding: 0px 10px; 
  color:#333;
  background: #F8F8F8 0% 0% no-repeat padding-box;
  top: 100%;
  overflow:hidden;
  height: 40px;
  box-shadow: 0px 4px 7px #00000000;
  border: 1px solid #E5E3E300;
  max-height: 0;
}

span:hover:before {
  animation-name: tooltip;
  animation-duration: 1.5s;
  animation-iteration-count: 1;
  animation-fill-mode: forwards;
}

@keyframes tooltip {
  0% {
    max-height: 0;
  }
  80% {
    max-height: 0;
    box-shadow: 0px 4px 7px #00000000;
    border: 1px solid #E5E3E300;
  }
  99% {
    max-height: 40px;
    box-shadow: 0px 4px 7px #00000035;
    border: 1px solid #E5E3E3;
  }
  100% {
    max-height: 40px;
    box-shadow: 0px 4px 7px #00000035;
    border: 1px solid #E5E3E3;
  }
}
```