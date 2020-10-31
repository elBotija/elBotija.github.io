---
layout: post
title: Formas de centrar un objeto con css 
---

Vamos a mostrar algunas formas para alinear un objeto 

```css
{
  position: absolute;
  left: calc(50% - 10px);
}
```

En esta forma utilizamos *calc* para restarle la mitad del ancho del objeto. De esta forma lo dejamos centrado ya que el punto de referencia para mover el objeto en este caso es el lado izquierdo. 

```css
{
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
} 
```

En esta forma estamos utilizando la propiedad *transform*, que nos permite trasladar el objeto en el eje X y lo desplazamos un 50% de su tamaño. Esto es muy útil cuando no sabemos el ancho que puede tener nuestro objeto, como es en el caso de una class reutilizable. 

 

Dejamos aca un [**Ejemplo**](https://jsfiddle.net/o9x4vhet/11/) 