# CLAUDE.md

Este archivo lo lee Claude cada vez que trabaja en esta carpeta, sin que se lo pidas.
Lo vas a llenar en la sesión. Por ahora trae solo las reglas que aplican desde el
primer minuto.

---

## 1. Qué es este proyecto y quién lo usa

*(Lo escribes tú en la sesión: dos líneas. Qué es la página, para quién es y cada
cuándo se usa.)*

## 2. De dónde sale cada cifra

Los datos de esta página viven en Supabase, en el proyecto `curso-EJEMPLO`.
Ninguna cifra ni ningún texto que se muestre se escribe a mano en el HTML: todo
sale de esas tablas o de lo que la persona escriba en los formularios.

### Tabla `pacientes`

| Columna | Qué guarda |
|---|---|
| `id` | Identificador único, lo pone la base sola |
| `creado_en` | Fecha y hora del registro, la pone la base sola |
| `nombre` | Nombre completo (obligatorio) |
| `telefono` | Teléfono de contacto |
| `correo` | Correo de contacto |
| `fecha_nacimiento` | Fecha de nacimiento |
| `notas` | Notas libres |

### Tabla `citas`

| Columna | Qué guarda |
|---|---|
| `id` | Identificador único, lo pone la base sola |
| `creado_en` | Cuándo se agendó, la pone la base sola |
| `paciente_id` | De quién es la cita. Si se borra el paciente, sus citas se borran con él |
| `fecha_hora` | Cuándo es la cita |
| `duracion_minutos` | Cuánto dura (60 por default) |
| `motivo` | Motivo de la consulta |
| `estado` | Solo acepta `agendada`, `atendida` o `cancelada` |
| `pagada` | Si el paciente ya pagó (sí/no). Nace en "no" |
| `pagado_en` | Cuándo se confirmó el pago |
| `monto` | Cuánto pagó. Es opcional, pero no puede ser negativo |

### Tabla `administradores`

La usa el módulo de súper administrador.

| Columna | Qué guarda |
|---|---|
| `id` | Identificador único, lo pone la base sola |
| `creado_en` | Cuándo se dio de alta, la pone la base sola |
| `nombre` | Nombre del administrador (obligatorio) |
| `correo` | Correo de contacto |
| `bloqueado` | Si está bloqueado (sí/no). Nace en "no" |
| `pagado_hasta` | Fecha hasta la que está cubierto su pago |

El semáforo de la pantalla se calcula así, y **el bloqueo manda sobre todo lo demás**:
`bloqueado` → **bloqueado**; si no, `pagado_hasta` vacía o ya pasada → **vencido**;
si no → **al corriente**. Registrar un pago también le quita el bloqueo.

> **El bloqueo es una demostración, no un cobro.** Como la plataforma no lleva cuentas
> ni contraseñas, bloquear a un administrador cambia su estado en la lista pero **no le
> impide entrar**: cualquiera con la liga sigue pasando. La página lo dice en pantalla,
> y ese aviso no se debe quitar mientras no haya cuentas de verdad. Para que el bloqueo
> cobre, haría falta Supabase Auth y cerrar las tablas a quien no haya iniciado sesión.

### Tabla `registros`

Es la del ejercicio anterior (un muro de mensajes con `nombre` y `mensaje`).
Sigue existiendo pero la página ya no la usa.

### Permisos, y por qué importan

En `pacientes` y `citas` **cualquiera puede leer, agregar, editar y borrar**.
Esto es a propósito: el panel del administrador no lleva contraseña, y sin esos
permisos no funcionaría.

> **Esta página es un ejercicio con datos inventados.** Con estos permisos,
> cualquiera que tenga la liga ve y modifica todo. **No se deben meter datos
> reales de pacientes.** Si algún día se va a usar con gente de verdad, primero
> hay que poner cuentas con contraseña (Supabase Auth) y cerrar la lectura a
> quien no haya iniciado sesión.

En `registros` los permisos siguen como estaban: leer y agregar sí, borrar y
editar no.

## 3. Cómo quiero que trabajes aquí

- Antes de un cambio grande, dame el plan por escrito y espera mi visto bueno.
- Un cambio a la vez. Enséñame qué cambió antes de escribirlo.
- Trabaja siempre en una rama. Nunca escribas commits directo sobre `main`.
- **Si tienes acceso a mi base de datos, enséñame el SQL antes de correrlo y espera mi
  respuesta.** Crear o borrar tablas, agregar o quitar columnas y cambiar permisos no se
  deshacen con una rama: en cuanto corren, ya está.

### Cómo se cierra CUALQUIER cambio, en CUALQUIER rama

Ningún cambio se queda a medias en una rama. **Ya di mi permiso de una vez y para
siempre**: no me vuelvas a preguntar si fusionar o si desplegar. Cada vez que termines
algo, en este orden:

1. **Commit y push** a la rama en la que trabajaste.
2. **Abre el pull request** de esa rama hacia `main`, explicando qué cambió y por qué.
3. **Fusiona el pull request a `main`.**
4. **Despliega a producción**, que son los dos lugares de la sección siguiente.
5. **Verifica que de verdad quedó**: la publicación en verde y la página abriendo.
   Nunca digas que quedó sin haberlo comprobado.
6. **Dime la liga y qué cambió.**

Si alguno de esos pasos te lo bloquean los permisos, **dilo claramente y dime qué clic
me toca dar a mí**. No lo escondas ni des por hecho que quedó.

### Dónde vive producción

**La página web.** Se publica en **GitHub Pages**, sola, con la receta
`.github/workflows/publicar.yml`, cada vez que algo llega a `main`.
Liga: <https://workcloudbusiness-cyber.github.io/EMC_REPO/>

> **Netlify todavía NO está conectado.** Existe el sitio
> `mi-pagina-servicio-de-nube-s7` en la cuenta, pero está vacío y no está enganchado a
> este repositorio, así que **hoy no hay nada que desplegar ahí**. Para conectarlo hay
> que entrar a <https://app.netlify.com/projects/mi-pagina-servicio-de-nube-s7>, darle
> **Link repository** → GitHub → `workcloudbusiness-cyber/EMC_REPO`, rama `main`, y
> dejar vacíos los campos de compilación (el `netlify.toml` ya los define). Ese clic lo
> tiene que dar una persona. **Mientras eso no pase, no digas que desplegaste a
> Netlify**: no sería cierto. En cuanto se conecte, Netlify publicará solo con cada
> fusión a `main`, igual que Pages.

**La base de datos (Supabase).** El proyecto es `curso-EJEMPLO`. **No hay un ambiente de
pruebas aparte: en el momento en que corres una migración, ya estás en producción.** Por
eso el SQL se enseña antes de correrlo, aunque el resto del cambio vaya en una rama. Y
por eso el orden importa: **primero la migración, después la fusión** del código que la
usa, para que la página nunca le pida a la base una columna que todavía no existe.

## 4. Lo que nunca debes hacer

- **Nunca escribas en esta carpeta una llave que empiece con `sb_secret_` o que
  diga `service_role`.** La única llave que puede estar aquí es la que empieza
  con `sb_publishable_`, que está hecha para andar a la vista.
- No inventes datos. Si algo no está en la tabla, que la página diga que no hay
  nada todavía, no un ejemplo.
- No borres el historial ni fuerces cambios sobre lo ya publicado.

## 5. Mi regla de verificación

*(La escribes tú en la sesión: con qué frase cierras lo que entregas y qué tiene
que ser cierto para que puedas publicarlo.)*

## 6. Cómo vuelvo a abrir esto

- El proyecto vive en este repositorio de GitHub.
- Se abre pidiéndole a Claude una sesión sobre este repo; no hace falta descargarlo.
- La página publicada está en <https://workcloudbusiness-cyber.github.io/EMC_REPO/>
  (GitHub Pages). Netlify todavía no está conectado; ver la sección 3.
- La base de datos está en supabase.com, en el proyecto `curso-EJEMPLO`.

> **Si la página deja de mostrar datos después de una semana sin usarla**, casi
> siempre es que el proyecto gratuito de Supabase se pausó. Se despierta con el
> botón **Resume project**.
