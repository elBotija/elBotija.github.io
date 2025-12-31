---
layout: post
title: "AWS: Entendiendo IAM sin morir en el intento"
description: "Guía definitiva para entender AWS IAM. Diferencias entre Usuarios, Grupos, Roles y Políticas para asegurar tu nube sin complicaciones."
tags: [AWS, IAM, Seguridad, Cloud]
---

Si alguna vez entraste a la consola de AWS y te sentiste completamente perdido o abrumado por la cantidad de opciones, no estás solo. IAM (Identity and Access Management) es, sin dudas, el servicio más importante de AWS, pero también uno de los más intimidantes al principio.

La mala noticia es que si lo ignorás, vas a terminar con agujeros de seguridad gigantes. La buena es que no es tan complicado si entendés los 4 conceptos clave.

Hoy vamos a desmitificar IAM para que puedas dormir tranquilo sabiendo que tu nube está segura.

## 1. Usuarios (Users): ¿Quién sos?

Un **Usuario IAM** representa a una persona o servicio que interactúa con AWS.

Pensalo como tu cuenta de Netflix. Vos tenés tu usuario, tu pareja el suyo. Cada uno tiene sus propias credenciales (password o access keys).

*   **Consejo de Seguridad:** No uses el usuario "root" (el email con el que creaste la cuenta) para el día a día. Creáte un usuario IAM con permisos de administrador si es necesario, y al usuario root activale **MFA (Autenticación de dos factores)** y guardá las credenciales bajo 7 llaves.

## 2. Grupos (Groups): Organizate

Manejar permisos usuario por usuario es un dolor de cabeza. Imaginate que entra un nuevo desarrollador al equipo. ¿Le vas a copiar y pegar los permisos de otro? No.

Los **Grupos** son conjuntos de usuarios. Vos le das permisos al grupo (ej: "Developers", "Admins", "Viewers") y metés a los usuarios adentro.

*   **Regla de oro:** Nunca le asignes permisos directos a un usuario. Asignalos a un grupo y meté al usuario en el grupo. Es mucho más prolijo.

## 3. Políticas (Policies): Las reglas del juego

Acá es donde se define qué se puede hacer y qué no. Una **Política** es un documento JSON que dice "Permitir tal acción en tal recurso".

Se ven así:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::mi-bucket-de-fotos"
    }
  ]
}
```

Traducido: "Permitir (`Allow`) listar los archivos (`s3:ListBucket`) solamente en el bucket `mi-bucket-de-fotos`".

AWS ya viene con muchas políticas pre-armadas (AWS Managed Policies) como `AdministratorAccess` o `AmazonS3ReadOnlyAccess`. Para empezar, usá esas.

## 4. Roles: El disfraz temporal

Este es el concepto que más confunde. Un **Rol** es una identidad que tiene permisos, pero **no tiene credenciales permanentes** (ni password ni keys).

Un Rol es como un sombrero o un disfraz que te ponés por un rato.
¿Quién usa roles?
*   **Servicios de AWS:** Una función Lambda que necesita leer de DynamoDB "asume" un rol con esos permisos.
*   **Usuarios federados:** Si te logueás con tu Google o el Active Directory de tu empresa, estás asumiendo un rol.
*   **Cross-account:** Para que un usuario de la Cuenta A pueda tocar cosas en la Cuenta B.

## En pocas palabras

*   **Usuario:** Una persona o servicio (credenciales permanentes).
*   **Grupo:** Bolsa de usuarios.
*   **Política:** El documento JSON con los permisos.
*   **Rol:** Permisos temporales que alguien (o algo) asume.

---

### Conclusión

IAM es la base de todo. Si entendés estos 4 pilares, ya tenés el 80% de la batalla ganada. No le tengas miedo, pero tenéle respeto. Empezá siempre dando el **mínimo privilegio necesario** (Least Privilege) y andá abriendo el grifo de a poco.

En el próximo post vamos a volver a Python para ver cómo automatizar esas tareas aburridas que te sacan tiempo. ¡Hasta la próxima!
