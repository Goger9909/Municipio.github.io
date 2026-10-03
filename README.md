# Municipio Activo

Portal municipal con inicio de sesión, registro y envío de reclamos.

## Para quienes visitan el sitio

Abre la **URL HTTPS del portal** que comparta la municipalidad. Regístrate o inicia sesión desde el navegador; no necesitas instalar nada ni conocer los datos de MySQL.

La dirección `localhost`, el archivo `frontend/index.html` y `file://` no son direcciones públicas: solo sirven para pruebas en el equipo donde se ejecuta el backend.

## Publicar el portal (una sola vez)

Publica la aplicación Spring Boot y el sitio juntos en Clever Cloud. Así el backend se conecta a MySQL de forma automática en el servidor. Las personas que visitan la URL no configuran conexiones ni reciben credenciales.

1. **Protege la base.** Cambia en Clever Cloud la contraseña de MySQL que se compartió en el chat. No la escribas en GitHub, el README ni los archivos del sitio.
2. En Clever Cloud, crea una aplicación Java Maven y selecciona Java `21`.
3. En la sección **Service Dependencies** de esa aplicación, vincula el complemento MySQL existente. Al vincularlo, Clever Cloud proporciona a la aplicación las variables privadas `MYSQL_ADDON_HOST`, `MYSQL_ADDON_PORT`, `MYSQL_ADDON_DB`, `MYSQL_ADDON_USER` y `MYSQL_ADDON_PASSWORD`.
4. En las variables de entorno de la aplicación agrega:

   | Variable | Valor |
   | --- | --- |
   | `SPRING_PROFILES_ACTIVE` | `cloud` |
   | `CC_JAVA_VERSION` | `21` |
   | `CC_RUN_COMMAND` | `java -jar backend/target/backend-0.0.1-SNAPSHOT.jar` |
   | `SESSION_COOKIE_SECURE` | `true` |

   Clever Cloud usa [`clevercloud/maven.json`](clevercloud/maven.json) para compilar y empaquetar el backend y el sitio.
5. Copia el Git deployment URL de la aplicación desde la sección **Information** de Clever Cloud. En PowerShell, desde la carpeta del proyecto, configura ese destino una sola vez y publica la rama actual:

   ```powershell
   git remote add clever <URL-GIT-DE-CLEVER-CLOUD>
   git push clever HEAD:master
   ```

   Sustituye el texto entre `< >` por el Git deployment URL que muestra tu aplicación. Si ya existe un remoto llamado `clever`, no repitas el primer comando. Solo se publican los cambios que ya están guardados en commits.
6. Al terminar el despliegue, abre la URL HTTPS que muestre Clever Cloud y compártela. **Esa es la dirección que usarán todos**; no compartas una dirección `localhost`.

Guías oficiales: [aplicaciones Java Maven](https://www.clever.cloud/developers/doc/deploy/applications/java/java-maven/) y [vincular una base de datos existente](https://www.clever.cloud/developers/doc/deploy/applications/java/java-maven/#linking-a-database-or-any-other-add-on-to-your-application).

La aplicación sirve el sitio y la API desde el mismo dominio y mantiene la sesión con una cookie segura. No necesitas Apache, otro servidor para el frontend ni configurar CORS.

## Ejecutar localmente (solo desarrollo)

En Windows, Java 21 y conexión a MySQL son necesarios. Ejecuta [`Iniciar Municipio.bat`](<Iniciar Municipio.bat>), introduce las credenciales privadas de MySQL cuando las solicite y deja abierta la ventana. Después entra en [http://localhost:8080/](http://localhost:8080/). Cierra el backend con `Ctrl+C`.

Este iniciador es para desarrollo; **los visitantes del sitio publicado no lo necesitan**. Apache/XAMPP puede mostrar `404` y `localhost:8080` rechaza la conexión si el backend local no está ejecutándose.

## Cuentas y base de datos

El registro público crea usuarios ciudadanos. Las contraseñas de usuario deben tener de 6 a 8 letras o números.

Si falta el esquema, ejecuta una vez [`crear-esquema-clever-cloud.sql`](frontend/Sql/crear-esquema-clever-cloud.sql) en la consola SQL de la base. Para asignar roles administrativos, primero registra las cuentas y luego sigue [`asignar-roles-municipales.sql`](frontend/Sql/asignar-roles-municipales.sql).

## Reclamos

El backend guarda los reclamos y las imágenes en el servidor. En plataformas con almacenamiento temporal, los archivos de `uploads/` pueden desaparecer cuando se reinicia la aplicación.

## Pruebas

Desde PowerShell, en la carpeta del proyecto:

```powershell
.\backend\mvnw.cmd -f .\backend\pom.xml test
```
