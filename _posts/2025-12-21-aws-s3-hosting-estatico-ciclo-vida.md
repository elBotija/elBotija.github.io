---
layout: post
title: "AWS: S3 más allá del almacenamiento de archivos"
---

Cuando empezamos con AWS, solemos ver a S3 (Simple Storage Service) como un simple "disco duro en la nube" donde tirar backups, imágenes de usuarios o logs. Y sí, sirve para eso, pero quedarnos ahí es desperdiciar gran parte de su potencial.

S3 es una navaja suiza que te permite resolver problemas de arquitectura complejos sin tocar un solo servidor. Hoy vamos a ver dos funcionalidades que todo desarrollador debería tener en su caja de herramientas: el hosting de sitios estáticos y las políticas de ciclo de vida.

## 1. Hosting de Sitios Estáticos: Tu web en minutos

¿Tenés una landing page, un portfolio o una app hecha en React/Vue/Angular? No necesitás configurar un servidor Nginx ni pagar una instancia EC2 que va a estar ociosa el 90% del tiempo. S3 puede servir tu contenido HTML, CSS y JS directamente al navegador.

### Cómo activarlo

1.  Creá un bucket (el nombre debe ser único globalmente).
2.  Andá a la pestaña **Properties** y bajá hasta el final donde dice "Static website hosting".
3.  Dale a **Edit**, activalo y especificá tu `index.html`.

### El truco de los permisos

Por defecto, S3 bloquea todo acceso público (y está bien que así sea). Para que tu sitio se vea, tenés que "abrir la tranquera" explícitamente.

Primero, desactivá "Block all public access" en la pestaña **Permissions**.
Segundo, agregá una **Bucket Policy** que permita leer los objetos a todo el mundo:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::tu-bucket-nombre/*"
        }
    ]
}
```

¡Listo! AWS te va a dar una URL tipo `http://tu-bucket-nombre.s3-website-us-east-1.amazonaws.com` y tu sitio ya está online. Rápido, barato y escala infinitamente.

> **Nota Pro:** Esto sirve perfecto para sitios simples o entornos de desarrollo. Si vas a producción o tenés una **Single Page Application (SPA)**, lo ideal es poner **CloudFront** por delante. Esto te da HTTPS, caché global y mejor performance. ¡No te preocupes, lo vamos a ver en detalle en un próximo artículo!

## 2. Ciclo de Vida: Ahorrá plata automáticamente

Uno de los problemas clásicos es acumular basura digital. Logs de hace 3 años, backups que ya no sirven, imágenes originales gigantes que ya procesaste. Guardar todo eso en la clase "Standard" de S3 cuesta dinero.

Las **Lifecycle Rules** son reglas que le decís a S3 para que mueva o borre archivos automáticamente según su antigüedad.

### Moviendo a clases más baratas

Podés configurar una regla que diga: "Si el archivo tiene más de 30 días, movelo a **S3 Standard-IA** (Infrequent Access)". Esta clase es más barata de almacenar, aunque te cobran un poquito más si accedés al archivo. Ideal para backups del mes pasado.

Si tenés datos que tenés que guardar por regulación pero nunca mirás (como logs de auditoría de hace un año), podés mandarlos a **S3 Glacier**. Es ridículamente barato, pero recuperar el archivo puede tardar horas.

### Borrado automático

¿Para qué querés los logs de debug de hace 6 meses? Para nada.

Configurá una regla de expiración:
*   **Prefijo:** `logs/`
*   **Acción:** Expire current versions of objects
*   **Días:** 90

Y olvidate. S3 va a pasar la escoba por vos todos los días, manteniendo tu bucket limpio y tu factura baja.

---

### Conclusión

S3 no es solo un lugar para guardar cosas; es un servicio inteligente que puede actuar como servidor web y como administrador de almacenamiento automatizado. Usar estas características no solo te ahorra tiempo de operación, sino que también cuida el bolsillo de tu proyecto.

En el próximo artículo vamos a volver a Python para hablar de algo que suele dar dolores de cabeza: el manejo de entornos virtuales con `venv` y `poetry`. ¡Hasta la próxima!
