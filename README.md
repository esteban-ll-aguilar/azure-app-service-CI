# azure-app-service-CI

## Despliegue automático (Azure Static Web Apps)

Este repositorio incluye un workflow de GitHub Actions para desplegar automáticamente el sitio en Azure Static Web Apps. El archivo relevante está en `.github/workflows/azure-static-web-apps-witty-bush-059aabf0f.yml`.

### Resumen rápido

- El workflow usa la acción oficial `Azure/static-web-apps-deploy@v1`.
- Se dispara en `push` a la rama `master` y en eventos de `pull_request` hacia `master`.
- Está configurado para un sitio estático sin paso de build (`skip_app_build: true`).

### Qué hace el workflow

- Clona el repositorio (`actions/checkout@v3`).
- Ejecuta la acción de Azure con el token de despliegue (secret de GitHub).
- Campos importantes en el YAML:
  - `azure_static_web_apps_api_token`: secret del token de despliegue (ver Prerrequisitos).
  - `app_location`: ruta del código fuente (en este repo está `/`).
  - `api_location`: ruta de funciones (vacío si no hay API).
  - `output_location`: carpeta resultante del build (vacía si no hay build).
  - `skip_app_build: true`: evita que la acción intente construir (útil para HTML/CSS/JS estático).

### Prerrequisitos

1. Tener creado un recurso **Azure Static Web App** en tu suscripción de Azure.
2. Obtener el **deployment token** del Static Web App (desde el portal de Azure o creado por el asistente de conexión con GitHub).
3. Añadir el token como secret en GitHub: `Settings  Secrets and variables  Actions  New repository secret`.
   - Nombre usado en este workflow: `AZURE_STATIC_WEB_APPS_API_TOKEN_WITTY_BUSH_059AABF0F` (puedes cambiar el YAML para usar otro nombre).
4. Confirmar que la rama objetivo en el YAML (`master`) coincide con tu flujo de trabajo. Si usas `main`, modifica el YAML.

### Pasos prácticos para habilitar despliegue automático

1. En Azure Portal: Crear un **Static Web App** o usar una existente.
   - Si conectas con GitHub desde el asistente, Azure puede configurar el workflow y el secret automáticamente.
2. Si no se configuró automáticamente: genera el deployment token en la página del recurso y cópialo.
3. En GitHub: añade un secret de Actions con el nombre empleado por el workflow y valor = deployment token.
4. Haz `git push` a `master` o crea/actualiza un PR hacia `master`.
5. En GitHub  Actions, revisa la ejecución del workflow y los logs.

### Adaptar el workflow para proyectos con build

- Para proyectos con build (React, Vue, Angular):
  - Quita `skip_app_build: true` o ponlo en `false`.
  - Ajusta `output_location` al directorio del build (por ejemplo `build` para Create React App, `dist` para Vite/Nuxt, `out`/`.next` según Next.js).
  - Mantén `app_location` en la carpeta donde está el código fuente si no está en la raíz.

Ejemplo (Create React App):

```yaml
app_location: "/"
api_location: ""
output_location: "build"
skip_app_build: false
```

### Problemas comunes y solución rápida

- Error de autenticación o token inválido: revisa el secret en GitHub y que el token corresponda al Static Web App.
- Workflow intenta hacer build y falla: configurar correctamente `skip_app_build` y `output_location`.
- Deploy finaliza pero sitio sin cambios: revisar logs y la URL de la Static Web App; verificar cache/CDN.
- Triggers en rama equivocada: cambia `master` por `main` si tu repo usa `main`.



---

Archivo del workflow: `.github/workflows/azure-static-web-apps-witty-bush-059aabf0f.yml`
