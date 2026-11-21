# PMI CV Platform: guía de instalación

Archivos:

- `index.html`: el programa completo. Súbelo a GitHub tal cual.
- `supabase.sql`: crea o actualiza la base de datos.
- Tus imágenes (fotos, logos, íconos): van en la misma carpeta que `index.html`.

---

## Paso 1. Actualizar la base de datos en Supabase (5 minutos)

1. Entra a <https://supabase.com/dashboard> y abre tu proyecto.
2. En el menú izquierdo, entra a **SQL Editor** y pulsa **New query**.
3. Abre `supabase.sql` con el Bloc de notas, copia **todo** y pégalo en el editor.
4. Pulsa **Run**. Debe aparecer *Success. No rows returned*. Los avisos (*NOTICE*) son normales.

Qué hace este archivo:

- Reemplaza las reglas de la versión sin login, que dejaban editar a cualquiera. Las hojas de vida que ya tenías **se conservan**: quedan "sin dueño" y solo tú puedes editarlas.
- Crea la tabla `perfiles` con los datos personales de cada usuario.
- Deja a `gusframos2014@gmail.com` como administrador.
- Puedes ejecutarlo otra vez cuando quieras: no borra datos.

Al final del archivo hay una limpieza **opcional** de las tablas del programa original (`profiles`, `experiences`, etc.). Viene desactivada a propósito: ejecútala solo si ya no las necesitas.

## Paso 2. Crear tu cuenta de administrador

1. Abre la página y pulsa **Iniciar sesión → Crear cuenta**.
2. Regístrate con `gusframos2014@gmail.com` y abre el enlace que llega a tu correo.

Ya tendrás el botón **Usuarios** arriba. Los permisos de administrador solo se activan **con el correo confirmado**.

Si el correo no llega: en Supabase entra a **Authentication → Users → Add user → Create new user**, escribe el correo y la contraseña, y marca **Auto Confirm User**.

## Paso 3. Correo propio (SMTP): necesario para que los correos lleguen a todos

El correo que trae Supabase por defecto **solo entrega mensajes a los miembros del equipo del proyecto** y envía muy pocos por hora. Sin un SMTP propio, tus usuarios no recibirán los enlaces de confirmación ni los de cambio de contraseña: verán el error *"Email address not authorized"*.

1. Crea una cuenta gratuita en un servicio de correo, por ejemplo **Brevo** (300 correos/día gratis) o **Resend**. También sirve Gmail con una "contraseña de aplicación".
2. Copia los datos SMTP que te da el servicio: servidor, puerto, usuario y contraseña.
3. En Supabase entra a **Authentication → Emails → SMTP Settings**, activa **Enable custom SMTP** y pega esos datos. En *Sender email* pon el remitente que verificaste en ese servicio.
4. En **Authentication → Rate Limits**, sube el límite de correos por hora si lo necesitas.

Con SMTP propio también puedes traducir los correos al español en **Authentication → Emails → Templates**.

## Paso 4. Publicar en GitHub Pages

1. En <https://github.com> crea un repositorio, por ejemplo `hojas-de-vida`, de tipo **Public**.
2. Pulsa **Add file → Upload files** y sube `index.html` junto con todas tus imágenes. El archivo debe llamarse exactamente `index.html`.
3. Entra a **Settings → Pages**. En *Source* elige **Deploy from a branch**, luego **main** y **/(root)**, y pulsa **Save**.
4. En uno o dos minutos la página quedará en `https://TU-USUARIO.github.io/hojas-de-vida/`.

## Paso 5. Avisarle a Supabase cuál es la dirección de tu página (obligatorio)

Sin este paso, los enlaces de los correos llevan a una página equivocada.

En Supabase entra a **Authentication → URL Configuration**:

- **Site URL:** `https://TU-USUARIO.github.io/hojas-de-vida/`
- **Redirect URLs** (pulsa *Add URL*):
  - `https://TU-USUARIO.github.io/hojas-de-vida/**`
  - Si pruebas en tu computador con un servidor local: `http://localhost:5500/**` (usa el puerto que corresponda).

Pulsa **Save**.

---

## Cómo funciona

| Quién | Qué puede hacer |
|---|---|
| Visitante sin cuenta | Ver el panel y todas las hojas de vida, imprimir y compartir. |
| Usuario registrado | Todo lo anterior. Además puede crear sus propias hojas de vida y editarlas o borrarlas (solo las suyas). En **Mi perfil** cambia sus datos personales, su correo y su contraseña. |
| Administrador (`gusframos2014@gmail.com`) | Ver, editar y borrar **todas** las hojas de vida. En **Usuarios** ve a todos los registrados, quién está **en línea ahora**, sus datos y sus hojas, y puede enviarle a cualquiera un correo para cambiar la contraseña. |

- **¿Olvidaste la contraseña?** En **Iniciar sesión → Olvidé mi clave**, escribe el correo y abre el enlace que llega. La página pide la contraseña nueva.
- **"En línea"** quiere decir que el usuario tiene la página abierta. La página avisa cada minuto. Si alguien cierra la pestaña, pasa a desconectado en unos 3 minutos.
- **Cambiar de administrador:** cambia el correo en la función `es_admin` de `supabase.sql` (y vuelve a ejecutarlo), y también en `ADMIN_EMAIL` dentro de `index.html`.
- **Eliminar un usuario:** en Supabase entra a **Authentication → Users**. Sus hojas de vida quedan "sin dueño" y tú las sigues administrando.
- **La clave que aparece en `index.html`** (`sb_publishable_…`) es pública por diseño. La seguridad la dan las reglas de `supabase.sql`.

## Imágenes

En la hoja de vida, las imágenes se escriben por su nombre de archivo, por ejemplo `foto.jpg`. Ese archivo debe estar en el repositorio, en la misma carpeta que `index.html`. Al pulsar **Exportar JSON**, la página indica exactamente qué archivos necesitas subir.

## Problemas frecuentes

| Mensaje | Solución |
|---|---|
| "Falta preparar la base de datos" | Ejecuta `supabase.sql` (Paso 1). |
| "Email address not authorized" | Configura el SMTP propio (Paso 3). |
| El enlace del correo abre otra página o `localhost` | Revisa la URL Configuration (Paso 5). |
| "El enlace no es válido o ya expiró" | Los enlaces se usan una sola vez y vencen. Pide uno nuevo. |
| Abriste `index.html` con doble clic y los enlaces del correo no vuelven | Los correos no pueden regresar a un archivo local. Usa la página de GitHub Pages. |
