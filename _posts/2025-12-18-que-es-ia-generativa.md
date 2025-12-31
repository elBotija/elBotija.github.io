---
layout: post
title: "IA: ¿Qué es la IA Generativa y por qué debería importarte?"
description: "Entendé qué es la IA Generativa (GenAI) y cómo está transformando el desarrollo de software. Productividad, tests y documentación automática."
tags: [IA, GenAI, Productividad, Desarrollo]
---

Seamos honestos: hasta hace no mucho tiempo atrás, cuando escuchábamos "Inteligencia Artificial", la mayoría de nosotros pensaba en clasificadores de imágenes, sistemas de recomendación o lógica compleja para NPCs en videojuegos. Era un campo reservado para Data Scientists con doctorados y máquinas potentes.

Pero la cosa cambió. Y cambió rápido. La IA Generativa (GenAI) no es solo una mejora incremental; es un cambio de paradigma. Ya no se trata solo de *analizar* datos existentes, sino de *crear* contenido nuevo. Y como desarrollador, si no le estás prestando atención, te estás perdiendo la herramienta más potente que apareció en años para potenciar tu productividad.

Hoy vamos a ver por qué esto es relevante para vos y cómo podés empezar a usarlo ya mismo para escribir menos código repetitivo y enfocarte en lo importante.

## 1. Generando el "Boilerplate" aburrido

¿Cuántas veces escribiste la estructura básica de una API, una clase de configuración o un script de migración? Es necesario, pero aburrido.

**Antes:**
Te pasabas 15 minutos copiando y pegando de StackOverflow o de un proyecto viejo, cambiando nombres de variables y rezando para no haberte olvidado de nada.

**Ahora:**
Le pedís a la IA que lo haga por vos.

> "Generame una estructura básica de FastAPI con un endpoint GET que devuelva un JSON con un mensaje de estado."

Y en segundos tenés:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/status")
def get_status():
    return {"status": "ok", "message": "Service is running"}
```

No es magia, es eficiencia. Vos seguís siendo el piloto, pero ahora tenés un copiloto que te alcanza las herramientas.

## 2. Documentación que no duele

A todos nos gusta el código limpio, pero a casi nadie le gusta escribir documentación. Es esa tarea que siempre queda para "el próximo sprint".

Con GenAI, podés pegar tu función y pedirle que genere el docstring.

**Tu código:**
```python
def calcular_descuento(precio, porcentaje):
    if porcentaje < 0 or porcentaje > 100:
        raise ValueError("Porcentaje inválido")
    return precio * (1 - porcentaje / 100)
```

**El prompt:**
"Generame un docstring estilo Google para esta función de Python."

**El resultado:**
```python
def calcular_descuento(precio, porcentaje):
    """Calcula el precio final después de aplicar un descuento.

    Args:
        precio (float): El precio original del producto.
        porcentaje (float): El porcentaje de descuento (0-100).

    Returns:
        float: El precio final con el descuento aplicado.

    Raises:
        ValueError: Si el porcentaje no está entre 0 y 100.
    """
    # ...
```

Listo. Documentación clara, estandarizada y sin perder tiempo.

## 3. Tests Unitarios en segundos

Escribir tests es fundamental, pero a veces da pereza pensar en todos los casos borde. La IA es excelente para esto.

Le pasás tu función y le decís: "Escribime tests unitarios usando `pytest` para esta función, incluyendo casos de error".

Y te devuelve:

```python
import pytest
from mi_modulo import calcular_descuento

def test_descuento_valido():
    assert calcular_descuento(100, 20) == 80.0

def test_sin_descuento():
    assert calcular_descuento(100, 0) == 100.0

def test_descuento_total():
    assert calcular_descuento(100, 100) == 0.0

def test_porcentaje_invalido():
    with pytest.raises(ValueError):
        calcular_descuento(100, 110)
```

Por supuesto, vos tenés que revisar que los tests tengan sentido, pero el trabajo pesado ya está hecho.

---

## 4. ¡Ojo! No le des las llaves de tu casa a boti

Esto es crítico. **Nunca, bajo ninguna circunstancia, pegues credenciales, API Keys, contraseñas o datos sensibles en un chat de IA** (especialmente en las versiones gratuitas o públicas).

Tené en cuenta que muchos de estos servicios usan tus interacciones para re-entrenar sus modelos. Si le pasás tu `AWS_SECRET_ACCESS_KEY` para que te ayude a debuggear, esa clave podría terminar siendo parte del conocimiento del modelo.

**Reglas de oro:**
1.  **Sanitizá tu código:** Antes de copiar y pegar, reemplazá cualquier secreto por placeholders como `"TU_API_KEY"`.
2.  **Leé los Términos y Condiciones:** Entendé cómo usa tus datos la herramienta que estás usando. Las versiones "Enterprise" suelen tener garantías de privacidad que las versiones "Free" no tienen.
3.  **Sentido común:** Si es información que no publicarías en StackOverflow, no se la des a la IA.

---

### Conclusión

La IA Generativa no viene a reemplazarte, viene a sacarte de encima las tareas repetitivas para que puedas dedicarte a resolver problemas complejos de arquitectura y lógica de negocio. Es una herramienta para ganar nivel de abstracción y velocidad.

En el próximo post volvemos a la nube para hablar de **AWS S3** y cómo usarlo para mucho más que guardar archivos. ¡Hasta la próxima!
