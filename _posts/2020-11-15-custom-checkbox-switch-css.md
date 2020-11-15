---
layout: post
title: Crear custom checkbox switch solo con CSS
---

Algunas veces necesitamos cambiar los estilos de los checkbox de un formulario, y podemos hacerlo solo con css.

[**Ejemplo**](https://jsfiddle.net/31ndfmsj/)

```html
<h1>Custom Checkboxes</h1>
<label class="container">
  <input type="checkbox" checked="checked">
  <span class="checkmark"></span>
</label>
```
```css
/* The container */
.container {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 12px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  min-height: 30px;
  
}

/* Hide the browser's default checkbox */
.container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
  
}

/* Create a custom checkbox */
.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 30px;
  width: 65px;
  background-color: #eee;
  border-radius:20px;
}

/* On mouse-over, add a grey background color */
.container:hover input ~ .checkmark {
  background-color: #ccc;
}

/* When the checkbox is checked, add a blue background */
.container input:checked ~ .checkmark {
  background-color: #2196F3;
}

/* Create the checkmark/indicator (hidden when not checked) */
.checkmark:after {
  content: "";
  position: absolute;
  transition: all .3s;
}

/* Show the checkmark when checked */
.container input:checked ~ .checkmark:after {
  display: block;
  left:calc(100% - 30px);
}

/* Style the checkmark/indicator */
.container .checkmark:after {
  left: 0px;
  top: 0px;
  width: 30px;
  height: 30px;
  border-radius:50%;
  background:red;
 
}
```