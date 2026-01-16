---
layout: post
title: "Python: Boto3, la librería que conecta Python con AWS"
description: "Aprendé a automatizar tu infraestructura en AWS usando Python y Boto3. Guía paso a paso para listar buckets y subir archivos sin entrar a la consola."
tags: [Python, AWS, Boto3, Automatización]
---

Si sos de los que todavía entra a la consola de AWS para crear buckets, subir archivos o reiniciar instancias, no pierdas mas tiempo tiempo...

La consola web es genial para explorar, pero cuando tenés que repetir tareas o manejar infraestructura seria, el click-click-click se vuelve tu peor enemigo. Acá es donde entra **Boto3**, el SDK de AWS para Python que te permite controlar prácticamente todo tu entorno cloud con código.

Hoy vamos a ver cómo dar tus primeros pasos con Boto3 para automatizar lo aburrido y repetitivo.

## Configuración inicial

Lo primero es tener la librería instalada. Asumo que ya tenés Python y `pip` listos (y si leíste mi [post anterior]({% post_url 2025-12-24-python-entornos-virtuales-venv-poetry %}), seguro estás usando un entorno virtual, ¿no?).

```bash
pip install boto3
```

También necesitás tener tus credenciales de AWS configuradas. Si seguiste la guía de [configuración de entorno]({% post_url 2025-12-12-configurando-entorno-desarrollo-aws %}), ya deberías tener tu perfil listo en `~/.aws/credentials`.

## 1. Conectando con la Nube

Para empezar a interactuar con AWS, necesitamos crear un "cliente" o "recurso". Boto3 maneja las credenciales automáticamente si las configuraste bien.

```python
import boto3

# Usamos 's3' como ejemplo, pero podría ser 'ec2', 'dynamodb', etc.
s3 = boto3.client('s3')

print("¡Conectado a S3!")
```

Si usás un perfil específico (recomendado para no usar la cuenta default por error):

```python
import boto3

session = boto3.Session(profile_name='mi-proyecto')
s3 = session.client('s3')
```

## 2. Listando tus Buckets

Vamos a hacer algo útil. Supongamos que querés ver qué buckets tenés creados en tu cuenta.

```python
import boto3

s3 = boto3.client('s3')

response = s3.list_buckets()

print("Mis Buckets:")
for bucket in response['Buckets']:
    print(f"- {bucket['Name']}")
```

La respuesta de `list_buckets()` es un diccionario gigante con metadata. Nosotros solo iteramos sobre la lista bajo la clave `Buckets` e imprimimos el nombre. Simple y directo.

## 3. Subiendo archivos (sin arrastrar y soltar)

Subir un archivo a S3 desde la consola es fácil, pero ¿qué pasa si tenés que subir 100 logs todos los días? Hacerlo con Python es mucho más eficiente.

```python
import boto3
import os

s3 = boto3.client('s3')

archivo_local = 'reporte-ventas.csv'
bucket_destino = 'mi-bucket-de-datos'
nombre_en_s3 = 'reportes/2026-01-15-ventas.csv'

try:
    s3.upload_file(archivo_local, bucket_destino, nombre_en_s3)
    print(f"¡Archivo {archivo_local} subido exitosamente a {bucket_destino}!")
except Exception as e:
    print(f"Ups, algo salió mal: {e}")
```

El método `upload_file` se encarga de todo: abre el archivo, lo transmite en partes si es grande y cierra la conexión. Vos solo le decís qué, dónde y cómo llamarlo.

---

### Conclusión

Automatizar tareas con Boto3 es un viaje de ida. Es cuestion de empezar para notar el tiempo que podemos ahorrar en tareas repetitivas.

En el próximo artículo vamos a hablar de **Asistentes de Código y Seguridad**, para que puedas escribir todo este código más rápido pero sin regalar tus credenciales en el proceso. ¡Hasta la próxima!
