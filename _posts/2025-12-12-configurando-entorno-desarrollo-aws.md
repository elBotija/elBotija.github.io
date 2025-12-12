---
layout: post
title: "AWS: Configurando tu entorno de desarrollo local para AWS"
---

Si trabajas con AWS, tarde o temprano vas a necesitar interactuar con la nube desde tu terminal. Ya sea para desplegar infraestructura, subir archivos a S3 o invocar una función Lambda, tener tu entorno local bien configurado es el primer paso para ser productivo.

En este post vamos a ver cómo configurar la AWS CLI, manejar múltiples perfiles y asegurar tus credenciales.

## 1. Instalando AWS CLI

Lo primero es tener la herramienta oficial. Dependiendo de tu sistema operativo, los pasos varían un poco:

### macOS
La forma más fácil es usando Homebrew:

```bash
brew install awscli
```

### Linux (x86_64)
Podés descargar e instalar el binario oficial:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

### Windows
La opción más directa es usar el instalador MSI. Podés descargarlo y ejecutarlo, o usar `msiexec` desde la terminal:

```cmd
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
```

Para verificar que todo quedó bien instalado (en cualquier OS):

```bash
aws --version
```

Deberías ver algo como `aws-cli/2.x.x ...`.

## 2. Configurando tus credenciales (La forma segura)

El error más común es usar un usuario "admin" para todo. **No lo hagas.**

Lo ideal es crear un usuario IAM con permisos programáticos específicos. Una vez que tengas tu `Access Key ID` y `Secret Access Key`, vamos a configurarlas.

En lugar de exportar variables de entorno manualmente cada vez, vamos a usar `aws configure`.

```bash
aws configure --profile mi-proyecto
```

Te va a pedir:
1.  **AWS Access Key ID**: Tu clave pública.
2.  **AWS Secret Access Key**: Tu clave privada (¡no la compartas!).
3.  **Default region name**: `us-east-1` (o la que uses).
4.  **Default output format**: `json`.

Notá que usé el flag `--profile mi-proyecto`. Esto es clave.

## 3. El poder de los Perfiles (Named Profiles)

Es muy probable que trabajes en más de un proyecto o tengas cuentas separadas para `dev`, `staging` y `prod`.

AWS CLI permite guardar múltiples configuraciones en el archivo `~/.aws/credentials`.

Para usar un perfil específico, podés agregar el flag `--profile` a cada comando:

```bash
aws s3 ls --profile mi-proyecto
```

O, mucho mejor, exportar la variable de entorno `AWS_PROFILE` para la sesión actual de tu terminal:

```bash
export AWS_PROFILE=mi-proyecto
```

Ahora todos los comandos que ejecutes usarán esas credenciales automáticamente.

## 4. SSO: El siguiente nivel

Si en tu empresa usan AWS SSO (Single Sign-On), la configuración cambia un poco pero es mucho más segura porque las credenciales son temporales.

```bash
aws configure sso
```

Este comando te guiará para loguearte vía navegador y autorizar a tu terminal.

## Resumen

1.  Instalá AWS CLI v2.
2.  Usá `aws configure --profile <nombre>` para separar entornos.
3.  Evitá usar las credenciales del usuario `root` o administradores generales.
4.  Acostumbrate a cambiar de contexto con `export AWS_PROFILE`.

¡Con esto ya estás listo para empezar a construir en la nube desde tu máquina! En el próximo post vamos a ver algunos trucos de Python que te van a facilitar la vida.
