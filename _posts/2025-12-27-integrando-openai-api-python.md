---
layout: post
title: "IA: Integrando OpenAI API con Python en 10 líneas"
---

Todo el mundo habla de Inteligencia Artificial, de ChatGPT y de cómo va a cambiar el mundo. Pero como desarrolladores, la pregunta que realmente nos importa es: ¿Cómo integro esto en mi código?

A veces pensamos que necesitamos conocimientos profundos de Machine Learning o servidores gigantescos para usar estos modelos. La realidad es que, gracias a las APIs modernas, agregar "inteligencia" a tus scripts es tan fácil como hacer una llamada HTTP.

Hoy vamos a ver cómo conectarte a GPT-4 usando Python en menos líneas de las que te imaginás.

## 1. Preparando el terreno

Lo único que necesitás es la librería oficial de OpenAI y una API Key. Si todavía no tenés una key, podés generarla en la plataforma de OpenAI.

Instalá la librería:

```bash
pip install openai
```

## 2. El Código

Vamos directo al grano. Este es el script mínimo viable para charlar con el modelo:

```python
from openai import OpenAI

# Inicializamos el cliente (busca la variable OPENAI_API_KEY por defecto)
client = OpenAI()

respuesta = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": "Sos un asistente útil y conciso."},
        {"role": "user", "content": "Explicame qué es una API en una frase."}
    ]
)

print(respuesta.choices[0].message.content)
```

¡Y listo! Eso es todo. Si contás las líneas de código real (sin comentarios ni espacios), son incluso menos de 10.

## 3. Entendiendo qué pasó

Desglosemos un poco lo que acabamos de hacer:

*   **`client = OpenAI()`**: Esto crea la conexión. Por defecto, la librería busca una variable de entorno llamada `OPENAI_API_KEY`. **Nunca** escribas tu clave directamente en el código (hardcode), es una mala práctica de seguridad. Usá variables de entorno.
*   **`model="gpt-4o"`**: Acá elegís el "cerebro". Podés usar `gpt-3.5-turbo` si querés algo más rápido y barato, o `gpt-4o` para mayor razonamiento.
*   **`messages`**: Es una lista que representa la conversación.
    *   `system`: Define el comportamiento o personalidad del modelo.
    *   `user`: Es lo que vos (o tu usuario) le pregunta.
*   **`respuesta.choices[0].message.content`**: La API devuelve un objeto complejo con mucha info (tokens usados, motivo de finalización, etc.). Lo que nos interesa suele estar en el contenido del primer mensaje de la lista de opciones (`choices`).

## 4. Hacelo dinámico

Obviamente, no vas a querer preguntar siempre lo mismo. Podés envolver esto en una función simple:

```python
def consultar_gpt(pregunta):
    completion = client.chat.completions.create(
        model="gpt-4o",
        messages=[
            {"role": "user", "content": pregunta}
        ]
    )
    return completion.choices[0].message.content

print(consultar_gpt("¿Cómo itero una lista en Python?"))
```

## 5. Un recordatorio importante: Uso Responsable

Antes de que salgas a conectar todo con GPT-4, una advertencia necesaria. El uso responsable implica dos grandes pilares:

1.  **No confíes ciegamente**: Las IAs pueden "alucinar" (inventar datos con total confianza). Si vas a usar esto en producción, asegurate de que siempre haya una capa de revisión humana. La IA es un copiloto, no el capitán del barco.
2.  **Cuidá tus datos**: Nunca envíes información sensible (contraseñas, datos personales de clientes, secretos comerciales) en los prompts. Recordá que estás mandando esa info a un servidor externo.

---

### Conclusión

Integrar IA en tus aplicaciones no tiene por qué ser complejo. Con unas pocas líneas de Python, podés empezar a aprovechar la potencia de los LLMs para resumir textos, clasificar información o generar contenido.

En el próximo artículo vamos a cambiar de tema y nos vamos a meter con la seguridad en la nube: **AWS IAM**, para entender usuarios y permisos sin morir en el intento.
