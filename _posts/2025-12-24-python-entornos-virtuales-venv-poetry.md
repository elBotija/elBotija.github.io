---
layout: post
title: "Python: Manejo de entornos virtuales con venv y poetry"
---

¿Alguna vez te pasó que un script funcionaba perfecto en tu máquina, pero al pasárselo a un compañero le tiraba error porque le faltaba una librería? ¿O que actualizaste una librería para un proyecto nuevo y rompiste uno viejo?

Si la respuesta es sí, es porque estás instalando todo globalmente. Y si la respuesta es no, probablemente ya usás entornos virtuales.

Hoy vamos a ver por qué `pip install` global es una mala idea y cómo arreglarlo con dos herramientas: el estándar `venv` y el moderno `poetry`.

## El problema del "Global Install"

Cuando hacés `pip install requests` sin un entorno virtual, esa librería se instala en el Python de tu sistema.

Imaginá que tenés:
*   **Proyecto A:** Usa `pandas` versión 1.0.
*   **Proyecto B:** Necesita `pandas` versión 2.0.

No podés tener las dos versiones instaladas globalmente al mismo tiempo. Al actualizar una, rompés el otro proyecto. Un caos.

## 1. La solución estándar: `venv`

Python ya trae una solución "de fábrica" llamada `venv`. Es simple, liviana y funciona en todos lados.

### Cómo usarlo

1.  **Crear el entorno:**
    Parate en la carpeta de tu proyecto y corré:
    ```bash
    python -m venv .venv
    ```
    Esto crea una carpeta `.venv` con una copia aislada de Python.

2.  **Activarlo:**
    *   En Mac/Linux:
        ```bash
        source .venv/bin/activate
        ```
    *   En Windows:
        ```cmd
        .venv\Scripts\activate
        ```
    Vas a ver que tu terminal ahora dice `(.venv)`.

3.  **Instalar cosas:**
    Ahora sí, dale sin miedo:
    ```bash
    pip install requests
    ```
    Todo lo que instales queda guardado *solo* en esa carpeta `.venv`.

4.  **Guardar las dependencias:**
    Para que otro pueda correr tu código, generá un archivo `requirements.txt`:
    ```bash
    pip freeze > requirements.txt
    ```
    Tu compañero solo tendrá que hacer `pip install -r requirements.txt` en su propio entorno.

## 2. La solución moderna: `poetry`

`venv` es genial, pero gestionar el `requirements.txt` a mano puede ser tedioso. ¿Qué pasa con las dependencias de las dependencias? ¿Cómo separo las librerías de desarrollo (como `pytest`) de las de producción?

Acá entra **Poetry**. Es una herramienta que maneja dependencias, empaquetado y entornos virtuales, todo en uno.

### Cómo usarlo

1.  **Iniciar un proyecto:**
    ```bash
    poetry init
    ```
    Esto te guía para crear un archivo `pyproject.toml`, que es el estándar moderno para configurar proyectos Python.

2.  **Agregar librerías:**
    Olvidate de pip. Ahora hacés:
    ```bash
    poetry add requests
    ```
    Poetry resuelve las versiones compatibles, instala todo y actualiza tu archivo de configuración automáticamente.

3.  **Correr scripts:**
    No hace falta "activar" nada manualmente. Podés correr tus scripts dentro del entorno de Poetry así:
    ```bash
    poetry run python mi_script.py
    ```

Poetry también crea un archivo `poetry.lock` que asegura que todos en tu equipo tengan *exactamente* las mismas versiones de todas las librerías. Cero sorpresas.

---

### Conclusión

Si estás haciendo un script rápido de un solo archivo, `venv` es tu mejor amigo: rápido y sin instalar nada extra.

Pero si estás armando un proyecto serio que vas a mantener en el tiempo o compartir con otros, **Poetry** te da una estructura profesional y te ahorra muchísimos dolores de cabeza a futuro.

En el próximo post vamos a ponernos el sombrero de IA para integrar la API de OpenAI en tus scripts con menos de 10 líneas de código.
