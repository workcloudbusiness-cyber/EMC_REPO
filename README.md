# Consultorio psicológico
Página de una sola pantalla con tres pestañas: el paciente pide su cita en **Registro del
paciente**; quien atiende ve el calendario, agenda y marca pagos en **Panel del
administrador**; en **Súper administrador** se dan de alta administradores y se ve quién
está al corriente, vencido o bloqueado. Es un ejercicio del curso Claude for Business hecho
sin escribir código. **Los datos son inventados: no metas datos reales de ningún paciente**,
porque las tablas están abiertas a cualquiera que tenga la liga.

## De dónde salen los datos
Ninguna cifra está escrita en el HTML. Todo sale de Supabase (proyecto `curso-EJEMPLO`): las
tablas `pacientes`, `citas` y `administradores`, que `index.html` consulta por su API con la
llave publicable. Las columnas de cada tabla están en `CLAUDE.md`, sección 2.

## Qué hay en .claude
`agents/revisor-antes-de-publicar.md` es un subagente que, antes de publicar, busca llaves
secretas, errores que tumben la página y código ineficiente, y entrega un reporte. No arregla
nada: el arreglo lo decides tú.

## Para continuar
Abre una sesión de Claude sobre este repositorio y pídele el cambio; el flujo (rama → pull
request → fusión a `main`) está en `CLAUDE.md`. La página se publica sola en
<https://workcloudbusiness-cyber.github.io/EMC_REPO/>, pero **hoy solo desde la rama
principal, que todavía no es `main`**; Netlify tampoco está conectado. Los dos, en `CLAUDE.md`.
