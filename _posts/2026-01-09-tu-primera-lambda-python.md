---
layout: post
title: "AWS: Tu primera función Lambda con Python"
description: "Tutorial paso a paso para crear, desplegar y probar tu primera función Lambda 'Hello World' en AWS usando Python. Entendé Serverless en 5 minutos."
tags: [AWS, Python, Serverless, Lambda, Tutorial]
---

Si venís del mundo tradicional de servidores, la idea de "no administrar servidores" suena raro. Pero una vez que probás **AWS Lambda**, se abre un mundo de posibilidades.

Imaginate escribir solo la lógica de tu negocio (una función Python) y que AWS se encargue de todo lo demás: escalar, parchear el sistema operativo, y lo mejor de todo: **cobrarte solo cuando tu código se está ejecutando**. Si nadie usa tu app a las 3 de la mañana, no pagás ni un centavo.

Hoy vamos a crear tu primera función Lambda. Nada de teoría aburrida, vamos...

## 1. La Consola de AWS

Primero, logueate en tu consola de AWS. En el buscador de arriba, escribí "Lambda" y entrá al servicio.

Hacé clic en el botón naranja que dice **"Create function"**.

Vas a ver varias opciones, elegí **"Author from scratch"** (es la que viene por defecto).

*   **Function name:** Ponele algo como `mi-primera-lambda`.
*   **Runtime:** Elegí **Python 3.12** (o la versión más nueva que veas).
*   **Architecture:** `x86_64` está bien para empezar.

Dale a **"Create function"** abajo de todo.

## 2. El Código (Handler)

AWS te va a llevar a la pantalla de configuración de tu nueva función. Bajá un poco hasta ver el editor de código (Code source).

Vas a ver un archivo `lambda_function.py` con algo así:

```python
import json

def lambda_handler(event, context):
    # TODO implement
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }
```

¿Qué es esto?
*   `event`: Es un diccionario con los datos que dispararon tu función (puede ser una petición HTTP, un archivo subido a S3, etc.).
*   `context`: Información sobre el entorno de ejecución (cuánta memoria tenés, cuánto tiempo te queda antes del timeout).

Vamos a cambiarlo un poco para que sea más interesante. Copiá y pegá esto:

```python
import json

def lambda_handler(event, context):
    nombre = event.get('nombre', 'Mundo')
    mensaje = f"¡Hola, {nombre}! Bienvenido a Serverless."
    
    print(f"Procesando saludo para: {nombre}") # Esto va a los logs de CloudWatch
    
    return {
        'statusCode': 200,
        'body': json.dumps(mensaje)
    }
```

Hacé clic en **"Deploy"** (el botón gris/naranja arriba del editor) para guardar los cambios. ¡Es clave este paso, si no deployás, estás corriendo la versión vieja!

## 3. Probando tu función

Ahora vamos a simular un evento. Hacé clic en el botón **"Test"**.

Se va a abrir una ventana para configurar un evento de prueba.
*   **Event name:** `PruebaPepe`
*   **Event JSON:** Cambiá el JSON por esto:

```json
{
  "nombre": "Pepe"
}
```

Dale a **"Save"** y después de nuevo a **"Test"**.

Deberías ver una caja verde que dice **"Execution result: succeeded"**. Si expandís los detalles, vas a ver la respuesta:

```json
{
  "statusCode": 200,
  "body": "\"¡Hola, Pepe! Bienvenido a Serverless.\""
}
```

¡Listo! Acabás de ejecutar código en la nube sin tocar un solo servidor.

## ¿Y ahora qué?

Esto es solo el comienzo. Lo potente de Lambda es que puede reaccionar a casi cualquier cosa que pase en tu cuenta de AWS. Algunos ejemplos de la vida real para que te inspires:

*   **Cron Jobs (EventBridge):** ¿Tenés un script que tiene que correr todos los días a las 8 AM para mandar un reporte? Configurás una regla de cron (Schedule Expression) y listo. Te olvidás de tener un servidor prendido las 24 horas solo para eso.
*   **Procesamiento de Colas (SQS):** Si tenés mucho tráfico, podés desacoplar tu aplicación mandando mensajes a una cola SQS y tener una Lambda que los vaya procesando a su ritmo (asíncronamente). Ideal para tareas pesadas que no querés que bloqueen al usuario.
*   **Orquestación (Step Functions):** ¿Y si necesitás encadenar varias funciones? Por ejemplo: una valida el pago, otra genera la factura y otra manda el mail. Con Step Functions podés coordinar todo ese flujo visualmente y manejar reintentos si algo falla.
*   **Reaccionar a Archivos (S3):** Cuando un archivo se sube a S3, una Lambda se despierte automáticamente, la redimensione, le ponga marca de agua y la guarde en otro bucket.

Las posibilidades son infinitas. En el próximo artículo vamos a ver cómo manejar los gastos y monitorear consumos en una cuenta de AWS.

¡A seguir codeando!
