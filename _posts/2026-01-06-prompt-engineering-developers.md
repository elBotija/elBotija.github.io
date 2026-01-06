---
layout: post
title: "IA: Prompt Engineering básico para desarrolladores"
description: "Aprendé a escribir prompts efectivos para programar. Técnicas de rol, contexto y chain-of-thought para que la IA genere mejor código Python y SQL."
tags: [IA, Prompt Engineering, Productividad]
---

Seguramente ya jugaste con ChatGPT, Claude o Gemini. Le pedís algo, te responde, te reís un rato. Pero cuando querés usarlo para programar en serio, a veces sentís que te da vueltas, te escribe código viejo o simplemente no entiende lo que querés.

El problema, la mayoría de las veces, no es la IA. Sos vos (bueno, tu prompt).

Hoy vamos a ver cómo hablarle a los modelos de lenguaje (LLMs) para que actúen como un Senior Developer y no como un pasante confundido.

## 1. El Contexto es Rey

Si le decís: *"Escribime una función para conectar a la base de datos"*, la IA tiene que adivinar:
*   ¿Qué lenguaje? (Python, Node, Go?)
*   ¿Qué base de datos? (Postgres, Mongo, MySQL?)
*   ¿Qué librería? (SQLAlchemy, Mongoose?)

**Prompt Malo:**
> "Hacé un script de conexión a DB."

**Prompt Bueno:**
> "Actuá como un experto en Python y AWS. Escribí una función usando la librería `boto3` para conectarse a una tabla de DynamoDB llamada 'Usuarios'. El código debe manejar excepciones y seguir las normas PEP8."

## 2. Dale un Rol (Persona)

Asignarle un rol ayuda a "setear" el tono y la calidad de la respuesta.

*   *"Actuá como un profesor de universidad..."* -> Te va a explicar los conceptos.
*   *"Actuá como un consultor de seguridad..."* -> Se va a enfocar en vulnerabilidades.
*   *"Actuá como un Senior Backend Engineer..."* -> Va a priorizar eficiencia y buenas prácticas.

## 3. Automatizá con Archivos de Reglas (Rules)

Si te encontrás repitiendo siempre lo mismo ("Usá snake_case", "No uses librerías externas"), es hora de automatizar.

La mayoría de los entornos de desarrollo modernos y asistentes de IA permiten definir un archivo de "Reglas" que el modelo lee antes de cada respuesta.

*   **VS Code / IDEs:** Podés configurar reglas a nivel de proyecto para que el asistente sepa exactamente cómo escribir código para ese repo.
*   **Claude Projects:** Podés subir un archivo `project_rules.md` o similar a tu "Project", y Claude lo va a respetar en cada chat dentro de ese proyecto.
*   **Amazon Q o Kiro:** Tiene su propio archivo de reglas que puedes configurar.

Por mencionar algunos, te invito a que busques en la documentación de tu herramienta de preferencia para ver cómo configurar estas reglas.

**¿Qué poner en estas reglas?**
*   Estándares de código (PEP8, ESLint).
*   Tecnologías preferidas (e.g., "Usá siempre `pytest` para tests").
*   Formatos de respuesta (e.g., "Solo código, sin explicaciones").

Esto te ahorra escribir el mismo "boilerplate" de prompt una y otra vez.

## 4. Chain of Thought (Cadena de Pensamiento)

Para problemas complejos, pedile que piense paso a paso. Esto reduce drásticamente las alucinaciones y errores lógicos.

**Ejemplo:**
> "Quiero migrar una base de datos de MySQL a DynamoDB. Antes de darme el código, explicame paso a paso tu estrategia de migración, considerando los tipos de datos y la capacidad de lectura/escritura."

## 5. Few-Shot Prompting (Dame ejemplos)

La mejor forma de que la IA entienda el formato que querés es dándole ejemplos.

**Prompt:**
> "Convertí estos nombres de variables de `snake_case` a `camelCase`.
>
> Entrada: nombre_usuario
> Salida: nombreUsuario
>
> Entrada: fecha_nacimiento
> Salida: fechaNacimiento
>
> Entrada: id_transaccion_bancaria
> Salida:"

La IA va a completar con `idTransaccionBancaria` casi con un 100% de certeza.

## 6. Iterá, no te quedes con la primera

Rara vez el primer prompt es perfecto. Tratalo como una conversación.
*"Ese código está bien, pero usaste una librería deprecada. Actualizalo a la versión 2.0."* o *"Hacelo más conciso."*

---

### Conclusión

El Prompt Engineering no es magia, es saber comunicarse con precisión. Cuanto más claro, específico y contextual sea tu pedido, mejor será el código que recibas.

Acordate: La IA es una herramienta multiplicadora. Si le das basura, te devuelve basura multiplicada. Si le das claridad, te devuelve productividad pura.

En el próximo post, vamos a poner esto en práctica y vamos a desplegar **tu primera función Lambda con Python**. ¡Prepará la terminal!
