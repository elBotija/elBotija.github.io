---
layout: post
title: Embed iframe de responsive con css
---

Como hacer para que un iframe por ejemplo de Youtube sea responsive, conservando el aspect ratio.

Tomaremos como ejemplo un video de Youtube, lo vamos a envolver en un div que contega la class embed-player.
No hace falta ajustar los parametros de tamaño que trae el codigo original que proporciona Youtube.

La magia ocurre en el div contenedor, que por medio de un overflow hidden y seteando el height en 0, nos permite agregar el alto del bloque de manera porsentual con respecto al ancho del div, de esta forma podemos mantener el aspect ratio 16:9 del video.

```html
  <div class="embed-player">
    <iframe  width="560" height="315" src="https://www.youtube.com/embed/WSLMN6g_Od4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  </div>
```
```css
.embed-player{
  position: relative;
  padding-bottom: 56.25%;
  height: 0;
  padding-top: 0;
  overflow: hidden;
}
.embed-player iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
```