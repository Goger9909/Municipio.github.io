# Municipio Serranoble

Sitio web estático del Municipio Serranoble, publicado con GitHub Pages.

## Publicación

La página principal del repositorio redirige a `frontend/index.html`, por lo que el sitio también abre si GitHub Pages está configurado para publicar desde la rama. El flujo de GitHub Actions publica directamente el contenido de `frontend/` cuando se actualiza `main`; también se puede iniciar manualmente desde la pestaña **Actions**. Si elegís GitHub Actions como origen en **Settings > Pages > Build and deployment**, el sitio publicado abre directamente la aplicación.

La URL del sitio de este repositorio es:

<https://goger9909.github.io/Municipio.github.io/>

Si es la primera publicación, en **Settings > Pages > Build and deployment** seleccioná **GitHub Actions** como origen.

## Funciones que requieren el backend

GitHub Pages sirve archivos estáticos; no ejecuta la aplicación Java ni proporciona una base de datos. La página principal y los recursos informativos se pueden visitar sin backend, pero iniciar sesión, registrarse y enviar reclamos requieren desplegar también `backend/` en un servidor.

Cuando exista una API pública con HTTPS, configurá su URL base en la etiqueta `meta` `municipio-api-base-url` de `frontend/index.html` y `frontend/login.html`, y en `reclamos-api-base-url` de `frontend/reclamos-municipal.html`. La API debe permitir el origen `https://goger9909.github.io` mediante la variable `FRONTEND_ORIGINS`. Para conservar las sesiones entre dominios, el perfil cloud del backend usa cookies `Secure` y `SameSite=None`; el navegador también debe permitir cookies de terceros para el dominio de la API.
