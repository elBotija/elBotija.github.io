---
layout: post
title: "IA: Asistentes de Código y Seguridad — Cómo Proteger tus Secretos de Verdad"
description: "Los archivos de ignorado no son suficientes. Aprendé estrategias reales para que tus API keys y credenciales nunca lleguen a los servidores de IA."
tags: [IA, Seguridad, Copilot, Cursor, Claude Code, Secretos, DevSecOps]
---

Seamos honestos: el consejo de "tené cuidado con lo que copiás y pegás" es de manual, pero en el mundo real, **no escala**. Un día estás cansado, copiás un config entero para debuggear y ¡pum!, tu API key está viajando a los servidores de OpenAI o Anthropic.

Pero hay un problema más grande que nadie menciona: **los archivos de ignorado no son garantía de nada**.

## La Falsa Seguridad de los Archivos de Ignorado

Muchos artículos te dicen: "Configurá los ignorados y listo". Por ejemplo, en **Cursor** creás un archivo `.cursorignore`. En **GitHub Copilot** configurás "Content Exclusion" en la web. En **Claude Code** usás `permissions.deny`.

El problema es el mismo en todos: **creer que esta configuración te blinda**.

```
Tu proyecto/
├── src/
├── .gitignore        ← "ignorame"
├── .cursorignore     ← "ignorame (solo Cursor)"
└── .env              ← EL ARCHIVO SIGUE EXISTIENDO
```

Estas configuraciones son una **sugerencia** que la herramienta debería respetar. Pero:

- ¿Y si hay un bug en la extensión?
- ¿Y si una actualización cambia el comportamiento?
- ¿Y si otra herramienta de IA no lo respeta?
- ¿Y si por error le das acceso completo al workspace?

**Estás confiando ciegamente en que el vendor respete las reglas.** En seguridad, eso no es aceptable.

## El Principio Real: Lo que No Existe, No Se Filtra

La única forma de garantizar que un secreto no viaje a un servidor de IA es que **no exista dentro del directorio del proyecto**. Punto.

```
❌ Enfoque débil:    Secreto existe + archivo de ignorado
✅ Enfoque seguro:   Secreto NO existe en el proyecto
```

## Estrategia 1: Variables de Entorno a Nivel OS

La solución más simple y efectiva: definir las variables en tu shell, fuera de cualquier proyecto.

### Configuración

```bash
# ~/.bashrc o ~/.zshrc (fuera de cualquier proyecto)
export MI_APP_API_KEY="sk-abc123xyz"
export MI_APP_DATABASE_URL="postgres://user:pass@host/db"
export MI_APP_AWS_SECRET="wJAAAAAAAAAAAAAAAAANG..."
```

```bash
# Recargar la configuración
source ~/.zshrc
```

### En tu código

```python
import os

# Directo, sin librerías adicionales
api_key = os.environ.get("MI_APP_API_KEY")
db_url = os.environ.get("MI_APP_DATABASE_URL")
```

```javascript
// Node.js
const apiKey = process.env.MI_APP_API_KEY
const dbUrl = process.env.MI_APP_DATABASE_URL
```

### Ventajas

- ✅ No hay ningún archivo con secretos en el proyecto
- ✅ La IA no puede ver lo que no existe
- ✅ Funciona con cualquier herramienta, lenguaje o framework
- ✅ Cero dependencias adicionales

### Desventajas

- Tenés que hacer `source ~/.zshrc` cuando agregás nuevas variables
- Menos portable entre máquinas (hay que configurar en cada una)

## Estrategia 2: Archivo .env Fuera del Proyecto

Si preferís mantener un archivo `.env` por organización, ponelo **fuera del directorio del proyecto**.

### Estructura

```
/home/tu-usuario/
├── secrets/                      ← Carpeta de secretos (fuera de proyectos)
│   ├── proyecto-a.env
│   ├── proyecto-b.env
│   └── cliente-x.env
│
└── proyectos/
    └── mi-app/                   ← Tu proyecto (lo que ve la IA)
        ├── src/
        │   └── config.py
        ├── .gitignore
        └── (SIN .env acá)
```

### Configuración en Python

```python
import os
from dotenv import load_dotenv

# Cargar desde ubicación externa explícita
load_dotenv(os.path.expanduser("~/secrets/mi-proyecto.env"))

# Usar normalmente
api_key = os.environ.get("API_KEY")
```

### Configuración en Node.js

```javascript
require('dotenv').config({ 
  path: require('os').homedir() + '/secrets/mi-proyecto.env' 
})

const apiKey = process.env.API_KEY
```

### Helper reutilizable

```python
# src/config.py
import os
from dotenv import load_dotenv
from pathlib import Path

def load_secrets(project_name: str):
    """Carga secretos desde ~/secrets/{project_name}.env"""
    secrets_path = Path.home() / "secrets" / f"{project_name}.env"
    
    if not secrets_path.exists():
        raise FileNotFoundError(f"No existe: {secrets_path}")
    
    load_dotenv(secrets_path)

# Uso
load_secrets("mi-proyecto")
api_key = os.environ.get("API_KEY")
```

## Estrategia 3: Secrets Manager (Nivel Empresarial)

Para equipos o proyectos en producción, la mejor opción es que los secretos **nunca toquen tu máquina**.

### AWS Secrets Manager

```python
import boto3
import json

def get_secret(secret_name: str) -> dict:
    client = boto3.client('secretsmanager')
    response = client.get_secret_value(SecretId=secret_name)
    return json.loads(response['SecretString'])

# Uso
secrets = get_secret("mi-app/produccion")
api_key = secrets["api_key"]
db_password = secrets["db_password"]
```

### HashiCorp Vault

```python
import hvac

client = hvac.Client(url='https://vault.tu-empresa.com')
client.token = os.environ.get("VAULT_TOKEN")  # Solo el token en el entorno

secret = client.secrets.kv.read_secret_version(path='mi-app/config')
api_key = secret['data']['data']['api_key']
```

### Ventajas

- ✅ Secretos nunca existen en tu máquina local
- ✅ Rotación automática de credenciales
- ✅ Auditoría de quién accede a qué
- ✅ Control de acceso granular

## Comparación de Estrategias

| Estrategia | Secreto en proyecto | IA puede verlo | Complejidad | Ideal para |
|------------|---------------------|----------------|-------------|------------|
| `.env` en proyecto + ignore | ✅ Sí existe | Depende del vendor | Baja | ❌ No recomendado |
| Variables en `~/.zshrc` | ❌ No | ❌ No | Baja | Desarrollo personal |
| `.env` fuera del proyecto | ❌ No | ❌ No | Media | Equipos pequeños |
| Secrets Manager | ❌ No | ❌ No | Alta | Producción / Enterprise |

## Capa Adicional: Licencias Pro

Además de sacar los secretos del proyecto, las licencias Pro ofrecen garantías contractuales de que tus datos no se usan para entrenar modelos.

| Herramienta | Tier | ¿Usa datos para entrenar? |
|-------------|------|---------------------------|
| Amazon Q Developer | **Pro** | ❌ No |
| Amazon Q Developer | Free | ✅ Sí (opt-out disponible) |
| GitHub Copilot | **Business/Enterprise** | ❌ No |
| GitHub Copilot | Individual | ✅ Sí por defecto |
| Cursor | **Pro** (privacy mode) | ❌ No |

**Importante:** Incluso con licencia Pro, los datos **sí viajan** a los servidores para procesarse. La diferencia es que no se retienen para entrenamiento. Si tu política de seguridad es "los secretos no salen de la red", necesitás las estrategias anteriores además de la licencia Pro.

## Capa de Repositorio: Pre-commit Hooks

Aunque saques los secretos del proyecto, los pre-commit hooks son una red de seguridad extra para cuando alguien del equipo comete un error.

### TruffleHog

```bash
# Instalación
brew install trufflehog  # macOS
pip install trufflehog   # pip

# Escanear repositorio
trufflehog git file://. --only-verified
```

### Configuración con pre-commit

```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/trufflesecurity/trufflehog
    rev: main
    hooks:
      - id: trufflehog
        entry: trufflehog git file://. --since-commit HEAD --fail
        
  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets
        args: ['--baseline', '.secrets.baseline']
```

```bash
pip install pre-commit
pre-commit install
```

Ahora cada `git commit` escanea automáticamente por secretos antes de permitir el commit.

## ¿Y las Herramientas de Sanitización como LLM Guard?

Herramientas como [LLM Guard](https://github.com/protectai/llm-guard) o [Presidio](https://github.com/microsoft/presidio) son excelentes, pero **no sirven para el flujo de trabajo en IDE**.

### ¿Por qué?

Cuando usás Cursor, Copilot o Claude Code, el flujo es:

```
VS Code/Cursor → [extensión que no controlás] → Servidores del vendor
```

Vos no hacés la llamada HTTP. No hay punto de intercepción donde meter LLM Guard.

### ¿Cuándo sí sirven?

| Caso de uso | ¿LLM Guard/Presidio sirve? |
|-------------|----------------------------|
| App web que llama a GPT API | ✅ Sí |
| Backend que usa Claude API | ✅ Sí |
| Chatbot interno de tu empresa | ✅ Sí |
| Vos programando en Cursor/Copilot | ❌ No |
| Script de clipboard (uso manual) | ✅ Sí |

Si estás **construyendo una aplicación** que usa IA, ahí sí te sirven. Para tu flujo de trabajo diario en el IDE, no.

## Archivos de Ignorado: Úsalos, Pero No Confíes Solo en Ellos

Dicho todo lo anterior, sí conviene configurarlos como capa adicional. Solo no los consideres tu única defensa.

### .cursorignore (Cursor)

```text
# Secretos y configuración sensible
.env*
config/secrets/
*.pem
*.key
credentials.json

# Datos locales
*.sqlite
*.db
```

### Claude Code (Configuración)

Claude Code usa un archivo de configuración para denegar permisos explícitamente.

```json
// .claude/config.json
{
  "permissions": {
    "deny": [
      ".env*",
      "secrets/**",
      "**/*.pem"
    ]
  }
}
```

### GitHub Copilot (Content Exclusion)

Para organizaciones, configurar en GitHub:
1. Organization Settings → Copilot → Content Exclusion
2. Agregar patrones:
   ```
   - "*.env"
   - "**/*.pem"
   - "**/secrets/**"
   ```

## Checklist Final

### Mínimo viable (desarrollo personal)

- [ ] Mover secretos a `~/.zshrc` o `~/.bashrc`
- [ ] Verificar que no haya `.env` dentro del proyecto
- [ ] Configurar ignorados en tu herramienta (`.cursorignore`, Content Exclusion, etc.)

### Recomendado (equipos)

- [ ] Todo lo anterior, más:
- [ ] Archivo `.env` en `~/secrets/` fuera del proyecto
- [ ] Pre-commit hooks con TruffleHog
- [ ] Licencia Pro de la herramienta que usen (Q Developer, Copilot, Cursor)

### Enterprise / Regulado

- [ ] Todo lo anterior, más:
- [ ] AWS Secrets Manager o HashiCorp Vault
- [ ] Políticas de rotación automática
- [ ] Auditoría de acceso a secretos

## Conclusión: Paranoia Justificada

En seguridad, la paranoia es una virtud. Los archivos de ignorado son convenientes, pero confiar ciegamente en ellos es ingenuo.

**La regla de oro:** Si un secreto existe en el directorio de tu proyecto, asumí que puede filtrarse. La única protección real es que no exista ahí en primer lugar.

El costo de implementar estas estrategias es mínimo. El costo de una API key de AWS filtrada puede ser de miles de dólares en minutos, o peor, una brecha de datos de tus clientes.

Elegí tu nivel de paranoia según tu contexto, pero al menos sacá los secretos del directorio del proyecto. Tu yo del futuro te lo va a agradecer.

---
