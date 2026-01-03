---
layout: post
title: "Python: Automatizando tareas aburridas con scripts de Python"
description: "Tutorial paso a paso para organizar archivos y automatizar tareas repetitivas usando Python (os, shutil) y Bash. Recuperá tu tiempo libre."
tags: [Python, Automatización, Scripts, Bash]
---

¿Alguna vez te encontraste haciendo la misma tarea repetitiva en tu computadora una y otra vez? Renombrar 50 archivos, mover reportes de una carpeta a otra, o limpiar tu escritorio que parece un campo de batalla digital.

Si lo hiciste más de tres veces, es hora de automatizarlo.

Python no solo sirve para hacer inteligencia artificial o backends complejos; es también el mejor amigo del administrador de sistemas (y del desarrollador perezoso, que es el mejor tipo de desarrollador). Hoy vamos a ver cómo usar los módulos `os` y `shutil` para recuperar tu tiempo libre.

## 1. El problema: El caos de la carpeta "Descargas"

Todos tenemos esa carpeta. Llenas de PDFs, instaladores `.dmg` o `.exe`, imágenes y zips que bajamos "por las dudas". Vamos a escribir un script que ordene este lío automáticamente.

## 2. Las herramientas: `os` y `shutil`

Python ya viene con las baterías incluidas. No necesitás instalar nada extra.

*   **`os`**: Nos permite interactuar con el sistema operativo (listar archivos, ver rutas).
*   **`shutil`**: Sirve para operaciones de alto nivel como mover o copiar archivos.

## 3. Manos a la obra: El Organizador Automático

Este script va a mirar una carpeta, identificar la extensión de cada archivo y moverlo a una subcarpeta correspondiente (ej: `imagenes`, `documentos`).

```python
import os
import shutil

# Definimos la carpeta a ordenar (cambiala por la tuya)
CARPETA_ORIGEN = '/Users/yourusername/Downloads'

# Mapeo de extensiones a carpetas
TIPO_ARCHIVO = {
    'imagenes': ['.jpg', '.jpeg', '.png', '.gif', '.svg'],
    'documentos': ['.pdf', '.docx', '.txt', '.xlsx', '.csv'],
    'instaladores': ['.dmg', '.pkg', '.exe', '.zip', '.rar']
}

def organizar_archivos():
    # Listamos todo lo que hay en la carpeta
    for archivo in os.listdir(CARPETA_ORIGEN):
        ruta_completa = os.path.join(CARPETA_ORIGEN, archivo)
        
        # Si es una carpeta, la ignoramos
        if os.path.isdir(ruta_completa):
            continue
            
        # Obtenemos la extensión (ej: .pdf)
        _, extension = os.path.splitext(archivo)
        extension = extension.lower()
        
        # Buscamos a dónde debe ir
        carpeta_destino = None
        for tipo, extensiones in TIPO_ARCHIVO.items():
            if extension in extensiones:
                carpeta_destino = tipo
                break
        
        # Si encontramos destino, movemos el archivo
        if carpeta_destino:
            # Creamos la subcarpeta si no existe
            ruta_destino_final = os.path.join(CARPETA_ORIGEN, carpeta_destino)
            os.makedirs(ruta_destino_final, exist_ok=True)
            
            # Movemos el archivo
            shutil.move(ruta_completa, os.path.join(ruta_destino_final, archivo))
            print(f"Movido: {archivo} -> {carpeta_destino}")

if __name__ == "__main__":
    organizar_archivos()
    print("¡Limpieza terminada!")
```

## 4. ¿Qué acabamos de hacer?

1.  **`os.listdir()`**: Nos dio la lista de nombres de archivo.
2.  **`os.path.splitext()`**: Separó el nombre de la extensión de forma segura.
3.  **`os.makedirs(..., exist_ok=True)`**: Creó la carpeta destino. El `exist_ok=True` es clave: si la carpeta ya existe, no tira error y sigue de largo.
4.  **`shutil.move()`**: Hizo el trabajo sucio de mover el archivo.

## 5. Llevándolo al siguiente nivel

Podrías programar este script para que corra todos los días con `cron` (en Mac/Linux) o el Programador de Tareas de Windows. Así, tu carpeta de descargas siempre va a estar impecable sin que muevas un dedo.

## Bonus Track: Para los amantes de Bash

Si sos de los que prefieren la terminal pura y dura, o simplemente querés ver otra forma de hacerlo, acá tenés la versión en **Bash**. Es menos "legible" para un principiante, pero increíblemente potente y concisa.

```bash
#!/bin/bash

CARPETA_ORIGEN="/Users/yourusername/Downloads"

cd "$CARPETA_ORIGEN" || exit

# Crear carpetas si no existen (-p evita error si ya existen)
mkdir -p imagenes documentos instaladores

# Mover archivos silenciando errores si no hay coincidencias (2>/dev/null)
mv *.jpg *.jpeg *.png *.gif *.svg imagenes/ 2>/dev/null
mv *.pdf *.docx *.txt *.xlsx *.csv documentos/ 2>/dev/null
mv *.dmg *.pkg *.exe *.zip *.rar instaladores/ 2>/dev/null

echo "¡Limpieza terminada!"
```

Como ves, en Bash aprovechamos la expansión de comodines (`*.jpg`) que hace la shell por nosotros. Es otra herramienta para la misma solución.

---

### Conclusión

Automatizar no se trata solo de ahorrar segundos, se trata de liberar tu carga mental. Esos pequeños scripts son los que te hacen sentir que la computadora trabaja para vos y no al revés.

En el próximo artículo vamos a meternos en el mundo del **Prompt Engineering**, para aprender a pedirle cosas a la IA y que te devuelva código de calidad a la primera.
