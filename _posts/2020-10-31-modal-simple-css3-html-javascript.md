---
layout: post
title: Crear modal simple con css3, html y javascript 
---

¿Por qué? ¡Porque podemos hacerlo nosotros! Es para un uso muy básico. 

El producto terminado [**ACA**](https://jsfiddle.net/bk9thcdp/30/)

HTML
```html
<a onclick="openLightModal()">
Click me!
</a>
<div class="light-modal">
  <div class="light-modal-content">
    <span class="light-modal-close" onclick="closeLightModal()">X</span>
    <h1>
      <a href="https://elbotija.github.io/" target="_blank">elBotija blog</a>
    </h1>
  </div>
</div>
```
CSS
```css
.light-modal{
  width:100vw;
  height: 100vh;
  display:block;
  background: rgba(0,0,0,.7);
  position:fixed;
  left:0;
  top:0;
  z-index: -1;
  opacity: 0;
  animation-fill-mode: forwards;
}
.light-modal-content{
  width:calc(80vw - 40px);
  height:calc(80vh - 40px);
  position: absolute;
  left:50%;
  top:50%;
  transform: translate(-50%, -50%);
  background: #888;
  display:block;
  padding:20px;
}
.light-modal-close {
  position: absolute;
  top:5px;
  right:5px;
  cursor: pointer;
}


.light-modal-ready {
    z-index: 20;
    animation-name: in;
    animation-duration: 0.3s;
    animation-iteration-count: 1;
  }

.light-modal-removing {
  z-index: -1;
  animation-name: out;
  animation-duration: 0.3s;
  animation-iteration-count: 1;
}

@keyframes out {
  0% {
    opacity: 1;
    z-index: 20;
  }
  99% {
    opacity: 0;
    z-index: 20;
  }
  100% {
    opacity: 0;
    z-index: -1;
  }
}
@keyframes in {
  0% {
    opacity: 0;
    z-index: 20;
  }
  99% {
    opacity: 1;
    z-index: 20;
  }
  100% {
    opacity: 1;
    z-index: 20;
  }
}
```

Javascript
```javascript
function openLightModal() {
	document.querySelector('.light-modal').classList.add('light-modal-ready');
  document.querySelector('.light-modal').classList.remove('light-modal-removing');
}

function closeLightModal() {
		document.querySelector('.light-modal').classList.remove('light-modal-ready');
  document.querySelector('.light-modal').classList.add('light-modal-removing');
}
```